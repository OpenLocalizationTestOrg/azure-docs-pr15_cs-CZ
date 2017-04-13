<properties
    pageTitle="Zálohování a obnovení pro systém SQL Server | Microsoft Azure"
    description="Popisuje zálohování a obnovení aspektech databází SQL Server na virtuálních počítačích Azure."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="jroth" />

# <a name="backup-and-restore-for-sql-server-in-azure-virtual-machines"></a>Zálohování a obnovení pro systém SQL Server ve virtuálních počítačích Azure

## <a name="overview"></a>Základní informace

Zálohování dat do databáze systému SQL Server je důležitou součástí strategie při ochraně před ztrátou dat z důvodu aplikace nebo uživatel chyby. To platí rovnoměrně z vnější pro systém SQL Server na virtuálních počítačích Azure (VMs) systém.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Pro systém SQL Server spuštěné v Azure VMs můžete použít nativního backup a obnovení techniky používající připojených disků pro cílovou záložních souborů. Je ale omezený počet disků, které můžete připojit k Azure virtuálního počítače v závislosti na [velikosti virtuálního počítače](virtual-machines-linux-sizes.md). Je také režijních Správa disků vzít v úvahu.

Začínající SQL Server 2014, můžete zálohování a obnovení k úložišti objektů Blob Microsoft Azure. SQL Server 2016 obsahuje také vylepšení pro tuto možnost. Kromě toho pro databáze soubory uložené v úložišti objektů Blob Microsoft Azure SQL serveru 2016 poskytuje možnost téměř okamžité záloh a pro rychlé obnoví použití Azure snímků. Tento článek obsahuje přehled o těchto možnostech a další informace najdete v [SQL Server zálohování a obnovení se službou úložiště objektů Blob Microsoft Azure](https://msdn.microsoft.com/library/jj919148.aspx).

>[AZURE.NOTE] Informace o možnosti pro zálohování obrovské databází najdete v článku [Více TB SQL Server databáze zálohování strategie pro Azure virtuálních počítačích](http://blogs.msdn.com/b/igorpag/archive/2015/07/28/multi-terabyte-sql-server-database-backup-strategies-for-azure-virtual-machines.aspx).

V následujících částech obsahují informace specifické pro různé verze SQL serveru podporované v Azure virtuálního počítače.

## <a name="sql-server-virtual-machines"></a>SQL Server virtuálních počítačích

Spuštěné instance serveru SQL Server je na Azure virtuálního počítače, souborů databáze již nacházejí na dat discích v Azure. Discích bloky v úložišti objektů Blob Azure. Tak důvody pro zálohování databáze a má trvat mírně změnit. Zvažte tyto skutečnosti: 

- Už nepotřebujete k zálohování databáze zajistit ochranu před selháním hardwaru nebo média, protože Microsoft Azure poskytuje tuto ochrana v rámci služby Microsoft Azure.

- Potřebujete k zálohování databáze zajistit ochranu proti chybám uživatele nebo pro účely archivace, regulačních důvodů, proč a účely správy.

- Záložní soubor můžete uložit přímo v Azure. Další informace najdete v tématu v následujících částech, které obsahují pokyny pro různé verze SQL serveru.

## <a name="sql-server-2016"></a>SQL Server 2016

Microsoft SQL serveru 2016 podporuje [zálohování a obnovení s objekty BLOB Azure](https://msdn.microsoft.com/library/jj919148.aspx) funkcí v SQL Server 2014. Ale zahrnuje tyto výhody:

| Rozšíření 2016               | Podrobnosti                          |
|---------------------|-------------------------------|
| **Prokládání**              | Při zálohování k úložišti objektů blob Microsoft Azure SQL serveru 2016 podporuje zálohování více objektů BLOB povolit zálohování velké databází, až do velikosti 12,8 TB.      |
| **Zálohy snímku**                | Použitím Azure snímky záložní soubor SQL Server snímek nabízí téměř okamžité zálohování a rychlé obnoví pro soubory databáze uložena pomocí služba úložiště objektů Blob Azure. Tato možnost umožňuje zjednodušit zálohování a obnovení zásad. Záložní soubor snímek taky podporuje bodu v době obnovení. Další informace najdete v článku [Zálohy snímek pro soubory databáze v Azure](https://msdn.microsoft.com/library/mt169363%28v=sql.130%29.aspx).   |
| **Spravovat záložní plánování**            | SQL Server spravovaných zálohování Azure nyní podporuje vlastní plány. Další informace najdete v tématu [Zálohování spravovaných SQL serveru k Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx).   |

Kurz výhod SQL serveru 2016 při použití úložiště objektů Blob Azure, najdete v článku [kurz: Služba úložiště objektů Blob Microsoft Azure pomocí databáze SQL serveru 2016](https://msdn.microsoft.com/library/dn466438.aspx).

## <a name="sql-server-2014"></a>SQL Server 2014

SQL Server 2014 zahrnuje tyto výhody:

1. **Zálohování a obnovení Azure**:

 - *Zálohování SQL serveru k adrese URL* nyní podporuje v SQL Server Management Studio. Možnost obecnějším údajům Azure je k dispozici, pokud chcete použít zálohování a obnovení úkolu nebo plánu Průvodce údržbou SQL Server Management Studio. Další informace najdete v tématu [Zálohování serveru SQL na adresu URL](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).
 - *SQL Server spravovaných zálohování Azure* má nové funkce, které umožňují správu automatické zálohování. Toto je užitečné především pro automatizaci záložní správy pro SQL Server 2014 instance spuštěné v počítači Azure. Další informace najdete v tématu [Zálohování spravovaných SQL serveru k Microsoft Azure](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx).
 - *Automatické zálohování* poskytuje další automatizaci automaticky povolíte *SQL Server spravovaných zálohování Azure* u všech stávajících, tak i nové databáze pro OM Server SQL v Azure. Další informace najdete v tématu [Automatické zálohování pro systém SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-automated-backup.md).
 - Základní informace o všechny možnosti pro zálohování 2014 serveru SQL Azure najdete v článku [SQL Server zálohování a obnovení se službou úložiště objektů Blob Microsoft Azure](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).

1. **Šifrování**: SQL Server 2014 podporuje šifrování dat při vytváření zálohy. Podporuje několik algoritmů šifrování a použití asymetrické klávesy nebo osf certifikát. Další informace najdete v článku [Zálohy šifrování](https://msdn.microsoft.com/library/dn449489%28v=sql.120%29.aspx).

## <a name="sql-server-2012"></a>SQL Server 2012

Podrobné informace o SQL serveru zálohování a obnovení ve verzi SQL Server 2012 najdete v tématu [zálohování a obnovení z databáze SQL Server (SQL Server 2012)](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx).

Spuštění v SQL Server 2012 SP1 kumulativní aktualizace 2, můžete zálohování a obnovení z úložiště objektů Blob Azure služby. Tato vylepšení mohou sloužit k obecnějším údajům databáze systému SQL Server na SQL serveru ve počítače virtuální Azure nebo místní instance. Další informace najdete v tématu [SQL Server zálohování a obnovení se službou úložiště objektů Blob Azure](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Výhody používání služba úložiště objektů Blob Azure patří možnost obejít 16 disku limit připojených disků Snadná správa přímé dostupnost záložního souboru do jiné instance aplikace instance serveru SQL Server Azure virtuální počítač se systémem nebo instance místní pro účely obnovení havárie nebo migrace. Kompletní seznam všech výhod používání služba úložiště objektů blob Azure záloh SQL serveru naleznete v části *výhod* v [SQL Server zálohování a obnovení se službou úložiště objektů Blob Azure](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Doporučený postup doporučení a informace o řešení problémů najdete v tématu [zálohování a obnovení doporučené postupy (služba úložiště objektů Blob Azure)](https://msdn.microsoft.com/library/jj919149%28v=sql.110%29.aspx).

## <a name="sql-server-2008"></a>SQL Server 2008

SQL Server zálohování a obnovení v SQL Server 2008 R2 nezdaří najdete v článku [zálohování a obnovování databází na serveru SQL Server (SQL Server 2008 R2)](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx).

SQL Server zálohování a obnovení systému SQL Server 2008 najdete v článku [zálohování a obnovování databází na serveru SQL Server (SQL Server 2008)](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx).

## <a name="next-steps"></a>Další kroky

Pokud plánujete nasazení serveru SQL Azure OM, můžete najít zřizovací podle pokynů v následující kurz: [vytváření SQL Server virtuálního počítače na Azure pomocí Správce prostředků Azure](virtual-machines-windows-portal-sql-server-provision.md).

Zálohování a obnovení lze k migraci dat, ale existuje potenciálně jednodušší cesty migraci dat pro SQL Server na OM Azure. Úplné informace o možnosti migrace a doporučeních najdete v článku [Migrace databáze SQL Server na OM Azure](virtual-machines-windows-migrate-sql.md).

Zobrazit další [zdroje pro systém SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-server-iaas-overview.md).
