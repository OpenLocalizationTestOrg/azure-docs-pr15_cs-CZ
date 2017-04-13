<properties
    pageTitle="Správa databáze SQL Azure pomocí Azure automatizace | Microsoft Azure"
    description="Informace o použití služby Azure automatizaci ke správě databáze Azure SQL ve velkém měřítku."
    services="sql-database, automation"
    documentationCenter=""
    authors="jodoglevy"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/26/2016"
    ms.author="jolevy"/>



#<a name="managing-azure-sql-databases-using-azure-automation"></a>Správa databáze SQL Azure pomocí automatizace Azure

Tento průvodce vám představí služby Azure automatizaci a jak ji můžete použít k zjednodušení správy databáze Azure SQL.


## <a name="what-is-azure-automation"></a>Co je Azure automatizaci?

[Automatizace Azure](https://azure.microsoft.com/services/automation/) je služby Azure zjednodušení správy cloudové prostřednictvím automatizaci procesů. Automatické Azure dlouho probíhajících, ručně, k chybám a často opakované úkoly automatické větší spolehlivosti, efektivity a hodnotu time to pro vaši organizaci.

Azure automatizaci poskytuje modul spuštění vysoce spolehlivý a snadno dostupné pracovního postupu, který změní vlastním potřebám narůstající velikostí vaší organizace. V Azure automatizaci procesů můžete být vykazuje postavu ručně, systémů 3rd výrobců nebo plánované intervalech tak, aby úkoly z toho důvodu přesně potřeby.

Snižovat provozní režijních a uvolnit tak IT / DevOps pedagogy zaměření na práci, která přidá firmy hodnota přesunutím úkoly správy cloudu, aby automaticky spouštět Azure automatizaci.


## <a name="how-can-azure-automation-help-manage-azure-sql-databases"></a>Jak lze Azure automatické plánování databáze Azure SQL?

Databáze SQL Azure dá ovládat v Azure automatizaci pomocí [rutin prostředí PowerShell databáze SQL Azure](https://msdn.microsoft.com/library/dn546723.aspx) , které jsou k dispozici v [Azure PowerShell nástroje](https://msdn.microsoft.com/library/azure/jj156055.aspx). Azure automatizaci má tyto rutiny prostředí PowerShell databáze SQL Azure k dispozici mimo pole tak, aby umožňuje provádět všechny úlohy správy vaší databáze SQL v rámci služby. Můžete taky spárování těchto rutin v Azure automatizaci pomocí rutin pro další služby Azure, k automatizaci složitých úkolů přes Azure službami a 3 systémy stran.

Azure automatizaci má taky možnost komunikovat s servery SQL přímo, zadáním příkazy SQL pomocí Powershellu.

[Automatizace Azure postupu runbook Galerie](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) obsahuje řadu produktu týmu a komunity runbooks začít automatizace správy databáze SQL Azure, jiné služby Azure a 3 systémy stran. Galerie runbooks patří:

 * [Spouštění dotazů SQL databázi SQL serveru](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
 * [Velikost ve svislém směru (nahoru nebo dolů) databáze SQL Azure na plán](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
 * [Pokud se svou databázi blíží maximální velikosti zkrátit tabulky SQL](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
 * [Pokud jsou velmi fragmentovány index tabulek v databázi SQL Azure](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy Azure automatizace a jak ji můžete použít ke správě databáze Azure SQL, tyto odkazy vedou na další informace o automatizaci Azure.

- [Přehled Azure automatizaci](../automation/automation-intro.md)
- [Můj první postupu runbook](../automation/automation-first-runbook-graphical.md)
- [Azure automatické učení mapy](https://azure.microsoft.com/documentation/learning-paths/automation/)
- [Azure automatizace: Vaše zástupce SQL v cloudu](https://azure.microsoft.com/blog/2014/06/26/azure-automation-your-sql-agent-in-the-cloud/) 
 
