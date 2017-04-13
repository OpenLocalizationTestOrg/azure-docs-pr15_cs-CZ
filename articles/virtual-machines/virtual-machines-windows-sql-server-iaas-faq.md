<properties
    pageTitle="SQL Server na virtuálních počítačích Azure nejčastější dotazy týkající se | Microsoft Azure"
    description="V tomto článku najdete odpovědi na nejčastější dotazy týkající se serverem SQL Azure VMs."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="sql-server-on-azure-virtual-machines-faq"></a>SQL Server na virtuálních počítačích Azure časté otázky

V tomto tématu najdete odpovědi na některé nejčastější dotazy týkající se serverem [SQL Server na virtuálních počítačích Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/).

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

1. **Jak vytvořit Azure virtuálního počítače se serverem SQL Server?**

    Můžete to udělat dvěma způsoby. Nejsnazším řešením je vytvořit virtuální počítač, který obsahuje SQL Server. Kurz na registruje k Azure a vytváření OM SQL pomocí portálu najdete v článku [poskytnutí virtuálního počítače serveru SQL Server Azure portálu](virtual-machines-windows-portal-sql-server-provision.md). Máte taky možnost Ruční instalace SQL serveru virtuálního počítače a opakované použití licenci místní se [Pohyblivostí licence až Software Assurance na Azure](https://azure.microsoft.com/pricing/license-mobility/).

1. **Jaký je rozdíl mezi SQL VMs a služba databáze SQL?**

    Entity spuštěných SQL Server Azure virtuální počítač není, která se liší od serverem SQL Server ve vzdáleném datacentra. [Databáze SQL](../sql-database/sql-database-technical-overview.md) naopak nabízí databáze jako služby. S databáze SQL nemáte přístup k stroje, které hostitelem vašich databází. Úplné porovnání, najdete v článku [Zvolte obláčkem možnost SQL Server: databáze SQL Azure (PaaS) nebo SQL Server na Azure VMs (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md).

1. **Jak můžu migrovat databázi SQL serveru místní do cloudu?**

    Nejprve vytvořte Azure virtuálního počítače s instanci systému SQL Server. Potom migrujte místní databáze na tuto instanci. Strategie migraci dat najdete v článku [Migrace databáze SQL serveru k serveru SQL Server Azure OM](virtual-machines-windows-migrate-sql.md).

2. **Můžete změnit nainstalované funkce nebo instalace druhou instanci systému SQL Server na stejné OM?**

    Ano. SQL Server instalační médium se nachází ve složce na disku **C** . Spusťte **Setup.exe** z tohoto umístění přidat nové instance serveru SQL Server nebo změnit další nainstalované funkce SQL serveru v počítači.

3. **Jak upgradovat na novou verzi/verzi serveru SQL Server Azure OM**

    V současné době není žádný upgradu pro systém SQL Server spuštěné v Azure OM. Vytvoření nového Azure virtuálního počítače s požadovanou verzi serveru SQL Server/edition a migrace databáze do nového serveru pomocí standardní [postupy migraci dat](virtual-machines-windows-migrate-sql.md).

4. **Jak můžu nainstalovat my licencovaného kopii serveru SQL Server na OM Azure?**

    Zkopírujte instalační médium SQL Server angličtině systému Windows Server a potom nainstalovat SQL Server na OM. Licencování důvodů, musí mít [Licenci mobilita prostřednictvím Software Assurance na Azure](https://azure.microsoft.com/pricing/license-mobility/).

5. **Budete muset platit SQL náklady OM, pokud ho jenom používá pro úsporném režimu/převzetí?**

    Pokud vytváříte OM SQL pomocí galerie, musí mít samostatnou licenci pro úsporném OM SQL a cen je stejný. Pokud ručně nainstalovat SQL Server na počítač virtuální se [Licenci pohyblivostí](https://azure.microsoft.com/pricing/license-mobility/)máte možnost, která bezplatné pasivní SQL instancemi pro překlopení. Zkontrolujte požadavky na omezení a.

6. **Jak aktualizace použijí na OM Server SQL?**

    Umožnění virtuálních počítačích ovládat na hostitelském počítači, včetně kdy a jak nainstalovat aktualizace. Operační systém můžete ručně použití aktualizací windows nebo můžete povolit službu plánování s názvem [Automatické opravy](virtual-machines-windows-classic-sql-automated-patching.md). Automatické instalace oprav všechny aktualizace, které jsou označeny jako důležité, včetně aktualizací systému SQL Server v této kategorii. Další volitelné aktualizace k SQL serveru musí být nainstalovaný ručně.

7. **Je možné nastavit konfigurace není vidět v galerii virtuálního počítače (pro příklad Windows 2008 R2 + SQL Server 2012)?**

    Ne. Virtuální počítač obrázky z galerie, které obsahují SQL serveru musíte vyberte jednu z uvedených obrázky.

9. **Jak nainstalovat SQL datové nástroje na můj OM Azure**

    Stažení a instalace SQL datové nástroje na [Microsoft SQL Server Data Tools – Business Intelligence for Visual Studio 2013 ](https://www.microsoft.com/en-us/download/details.aspx?id=42313).

## <a name="resources"></a>Zdroje informací

Základní informace o serveru SQL Server na virtuálních počítačích Azure podívejte se na video [Azure OM je nejlepší platformu pro SQL Server 2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016). Otevřete dobré Úvod taky v tématu [SQL Server na virtuálních počítačích Azure přehled](virtual-machines-windows-sql-server-iaas-overview.md).

Další zdroje informací:

- [Zřízení virtuálního počítače serveru SQL Server Azure portálu](virtual-machines-windows-portal-sql-server-provision.md)
- [Migrace databáze na serveru SQL Server na Azure OM](virtual-machines-windows-migrate-sql.md)
- [Dostupnost a obnovení pro SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-high-availability-dr.md)
- [Výkon doporučené postupy pro systém SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-performance.md)
- [Vzorky aplikace a vývoj strategie pro SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)
