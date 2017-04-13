<properties
    pageTitle="Základní informace o serveru SQL Server na virtuálních počítačích Azure | Microsoft Azure"
    description="Informace o tom, jak spustit úplné edice systému SQL Server Azure virtuálních počítačích. Pokud potřebujete přímé odkazy na všechny obrázky OM SQL serveru a související obsah."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/19/2016"
    ms.author="jroth"/>

# <a name="overview-of-sql-server-on-azure-virtual-machines"></a>Základní informace o serveru SQL Server na virtuálních počítačích Azure

Toto téma popisuje možnosti pro systém SQL Server na virtuálních počítačích Azure (VMs) spolu s [odkazy na portálu obrázky](#option-1-create-a-sql-vm-with-per-minute-licensing) a základní informace o [běžných úkolů](#manage-your-sql-vm).

>[AZURE.NOTE] Pokud jste už znáte SQL serveru a jenom zajímají Naučte se nasadit OM serveru SQL, najdete v článku [poskytnutí virtuálního počítače serveru SQL Server Azure portálu](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="overview"></a>Základní informace
Pokud jste správce databáze nebo vývojář, Azure VMs lze přesunout místního serveru SQL Server úloh a aplikací do cloudu. Následující video poskytuje technický přehled SQL Server Azure VMs.

> [AZURE.VIDEO data-driven-sql-server-2016-azure-vm-is-the-best-platform-for-sql-server-2016]

Video obsahuje následující témata:

|Čas|Oblast|
|---|---|
| 00:21 | Jaké jsou Azure VMs? |
| 01:45 | Zabezpečení |
| 02:50 | Připojení |
| 03:30. | Úložiště spolehlivosti a výkonu |
| 05:20 | OM velikosti |
| 05:54 | Dostupnost a SLA |
| 07:30. | Konfigurace podpory |
| 08:00 | Sledování |
| 08:32 | Ukázka: Vytvoření OM 2016 serveru SQL |

>[AZURE.NOTE] Video se zaměřuje na SQL serveru 2016, ale Azure obsahuje obrázky OM pro mnoho verze systému SQL Server, včetně 2008, 2012, 2014 a 2016. 

## <a name="scenarios"></a>Scénáře
Existuje řada důvodů, které je možné provozovat dat v Azure. Při přesunutí aplikace na Azure zlepšuje výkon taky přesunout data. Ale existují další výhody. Automaticky máte přístup k několika datacentrech globální informace o stavu a havárie obnovení. Data jsou také vysoce zabezpečené a trvalé.

SQL Server Azure VMs se systémem je jednou z možností pro ukládání relačních dat v Azure. Je vhodné pro několik příčin. Můžete třeba konfigurovat OM Azure nejblíže k počítači serveru SQL Server místní podobně. Nebo můžete chtít spustit dalších aplikací a služeb na stejném databázovém serveru. Existují dva hlavní prostředky, které vám představit prostřednictvím ještě další scénáře a informace, které můžou pomoct:

 - [SQL Server na virtuálních počítačích Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/) přehled nejlepší scénáře pro používání serveru SQL Server Azure VMs. 
 - [Zvolte obláčkem možnost SQL Server: databáze SQL Azure (PaaS) nebo SQL Server na Azure VMs (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md) poskytuje podrobné porovnání databáze a SQL serveru SQL Server virtuálního počítače se systémem.

## <a name="create-a-new-sql-vm"></a>Vytvoření nového OM SQL
Následující části obsahují přímé odkazy na portálu Azure SQL Server virtuálního počítače Galerie obrázků. V závislosti na obrázek, který vyberete můžete buď platu pro systém SQL Server licencování náklady na základě za minutu nebo můžete přenést i vlastní licence (BYOL).

Podrobné pokyny pro tento proces najdete v kurzu [poskytování virtuálního počítače serveru SQL Server Azure portálu](virtual-machines-windows-portal-sql-server-provision.md). Zkontrolujte [výkon doporučené postupy pro SQL Server VMs](virtual-machines-windows-sql-performance.md), která vysvětluje postup při výběru velikosti odpovídající počítače a dalších funkcí dostupných během vytváření.

## <a name="option-1-create-a-sql-vm-with-per-minute-licensing"></a>Možnost 1: Vytvoření OM SQL s licencí za minutu
Následující tabulka obsahuje matici dostupné SQL Server obrázků v galerii virtuálního počítače. Klikněte na jakýkoli odkaz na začít vytvářet nový OM SQL s zadané verzi, edition a operační systém.

|Verze|Operační systém|Edition|
|---|---|---|
|**SQL 2016**|Windows serveru 2012 R2|[Pole organizace](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMEnterpriseWindowsServer2012R2), [Standardní](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMStandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMWebWindowsServer2012R2), [vývoj](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMDeveloperWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMExpressWindowsServer2012R2)|
|**SQL 2014 SP1**|Windows serveru 2012 R2|[Pole organizace](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1EnterpriseWindowsServer2012R2), [Standardní](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1ExpressWindowsServer2012R2)|
|**SQL 2014**|Windows serveru 2012 R2|[Pole organizace](https://portal.azure.com/#create/Microsoft.SQLServer2014EnterpriseWindowsServer2012R2), [Standardní](https://portal.azure.com/#create/Microsoft.SQLServer2014StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014WebWindowsServer2012R2)|
|**SQL 2012 SP3**|Windows serveru 2012 R2|[Pole organizace](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [Standardní](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows serveru 2012 R2|[Pole organizace](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012R2), [Standardní](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012|[Pole organizace](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012), [Standardní](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2ExpressWindowsServer2012)|
|**SQL 2008 R2 S AKTUALIZACÍ SP3**|Windows Server 2008 R2|[Pole organizace](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3EnterpriseWindowsServer2008R2), [Standardní](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3StandardWindowsServer2008R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3WebWindowsServer2008R2)|
|**SQL 2008 R2 S AKTUALIZACÍ SP3**|Windows Server 2012|[Express](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3ExpressWindowsServer2012)|

## <a name="option-2-create-a-sql-vm-with-an-existing-license"></a>Možnost 2: Vytvoření OM SQL pomocí stávající licence
Můžete přenést i vlastní licence (BYOL). V tomto scénáři je pouze zaplatit OM bez žádné další poplatky licencování SQL serveru. Vlastní licenci, použijte matice verze SQL serveru, edice a operační systémy dole. Na portálu jsou tyto názvy obrázek předponou **{BYOL}**.

|Verze|Operační systém|Edition|
|---|---|---|
|**SQL Server 2016**|Windows serveru 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2), [Standardní BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2)|
|**SQL Server 2014 SP1**|Windows serveru 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1EnterpriseWindowsServer2012R2), [Standardní BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1StandardWindowsServer2012R2)|
|**SQL Server 2012 SP2**|Windows serveru 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [Standardní BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2)|

> [AZURE.IMPORTANT] Použití obrázků s BYOL OM, pokud nemáte a Enterprise Agreement se [Pohyblivostí licence až Software Assurance na Azure](https://azure.microsoft.com/pricing/license-mobility/). Potřebujete ještě platnou licenci pro verzi/edice systému SQL Server, který chcete použít. Do **10** dnů zřizování vaší OM, musíte [Zadejte potřebné informace BYOL společnosti Microsoft](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf) .

## <a name="manage-your-sql-vm"></a>Správa OM SQL
Po zajištění služby vaší OM SQL serveru, existuje několik volitelné správy úkolů. V mnoha aspekty konfigurace a Správa serveru SQL Server přesně tak, jako by správě instance služby SQL Server místní. Některé úkoly jsou však specifické pro Azure. V následujících částech zaměřuje na některé z těchto oblastí s odkazy na další informace.

### <a name="connect-to-the-vm"></a>Připojení k OM
Základní kroky správy reprodukujte se připojit k vaší OM serveru SQL pomocí nástroje, například SQL Server Management Studio (SSMS). Pokyny pro připojení k vaší nové OM SQL serveru najdete v článku [připojení k SQL serveru virtuálního počítače na Azure](virtual-machines-windows-sql-connect.md).

### <a name="migrate-your-data"></a>Přenést data
Pokud máte existující databázi, je vhodné, přejděte do nově vytvořený OM SQL. Seznam možnosti migrace a pokyny najdete v tématu [Migrace databáze SQL Server na OM Azure](virtual-machines-windows-migrate-sql.md).

### <a name="configure-high-availability"></a>Konfigurace dostupnost
Pokud požadujete dostupnost, zvažte možnost konfigurace SQL Server dostupnost skupiny. Tento postup představuje více Azure VMs v síti virtuální. Portál Azure obsahuje šablonu, která nastavuje této konfiguraci za vás. Další informace najdete v tématu [Konfigurace skupiny dostupnosti AlwaysOn v správce prostředků Azure virtuálních počítačích](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). Pokud chcete ručně nakonfigurovat skupiny dostupnosti a související posluchače, najdete v článku [Konfigurace AlwaysOn dostupnost skupin v Azure OM](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Další úvahy dostupnost najdete v článku [dostupnost a obnovení pro systém SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-high-availability-dr.md).

### <a name="back-up-your-data"></a>Zálohování dat aplikace
Azure VMs využít [Automatické zálohy](virtual-machines-windows-sql-automated-backup.md), která pravidelně vytvoří zálohy databáze k úložišti objektů blob. Můžete taky ručně použít tento postup. Další informace najdete v tématu [Použití Azure úložiště pro SQL Server zálohování a obnovení](virtual-machines-windows-use-storage-sql-server-backup-restore.md). Přehled všechny možnosti zálohování a obnovení naleznete v tématu [zálohování a obnovení pro systém SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-backup-recovery.md).

### <a name="automate-updates"></a>Automatické aktualizace
Azure VMs pomocí [Automatické opravy](virtual-machines-windows-sql-automated-patching.md) můžete naplánovat údržby při instalaci důležité windows a SQL Server automaticky aktualizuje.

### <a name="customer-experience-improvement-program-ceip"></a>Program Zlepšování softwaru prostředí (program)
Program zlepšování softwaru na základě zkušeností zákazníků (a) aktivované ve výchozím nastavení. To odesílá společnosti Microsoft pomáhají zlepšit SQL serveru sestav. Neexistuje žádný úlohy správy povinné se program, pokud chcete zakázat po zajištění služby. Můžete přizpůsobit nebo zakázání OKNĚ připojením k OM Vzdálená plocha. Spusťte nástroj **Chyba serveru SQL a vytváření sestav použití** . Postupujte podle pokynů jak zakázat vytváření sestav. 

Další informace naleznete v části program témat [Přijměte licenční podmínky](https://msdn.microsoft.com/library/ms143343.aspx) . 

## <a name="next-steps"></a>Další kroky
[Prozkoumání Naučná stezka](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) pro systém SQL Server na virtuálních počítačích Azure.

Další otázky? Přečtěte si nejprve, [SQL Server na virtuálních počítačích Azure časté otázky](virtual-machines-windows-sql-server-iaas-faq.md). Ho ale přidat dotazy nebo komentáře na konec všech témat SQL OM chcete provést interakci s Microsoft a komunity.
