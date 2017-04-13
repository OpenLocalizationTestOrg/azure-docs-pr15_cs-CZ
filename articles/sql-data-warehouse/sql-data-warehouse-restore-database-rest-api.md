<properties
   pageTitle="Obnovení Azure SQL datový sklad (REST API) | Microsoft Azure"
   description="Úkoly rozhraní REST API pro obnovení datový sklad SQL Azure."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/21/2016"
   ms.author="lakshmir;barbkess;sonyama"/>

# <a name="restore-an-azure-sql-data-warehouse-rest-api"></a>Obnovení Azure SQL datový sklad (REST API)

> [AZURE.SELECTOR]
- [Základní informace][]
- [Portál][]
- [Prostředí PowerShell][]
- [ZBÝVAJÍCÍ][]

V tomto článku se dozvíte, jak obnovit sklad dat SQL Azure pomocí rozhraní REST API.

## <a name="before-you-begin"></a>Než začnete

**Ověřte DTU kapacity.** Každý SQL datový sklad je hostovaný aplikací SQL server (například myserver.database.windows.net), který má výchozí kvóta DTU.  Před obnovením datový sklad SQL, ověřte, že serveru SQL server má dost zbývající DTU kvóty pro databáze obnovena. Zjistěte, jak vypočítat DTU potřeby nebo požádejte o další DTU, najdete v článku [požadavek na změnu DTU kvóty][].

## <a name="restore-an-active-or-paused-database"></a>Obnovení databáze aktivní nebo pozastaveném ukládání

Obnovení databáze:

1. Pokud potřebujete seznam míst obnovení databáze pomocí operace body obnovení databáze získat.
2. Začněte myší [obnovení databáze vytvořit žádost o][] obnovení.
3. Sledování stavu obnovení pomocí operace [stav operace databáze][] .

>[AZURE.NOTE] Po dokončení obnovení můžete nakonfigurovat obnoveného databázi tak, že následující [Konfigurace databáze po obnovení][].

## <a name="restore-a-deleted-database"></a>Obnovení odstraněné databáze

Obnovení odstraněné databáze:

1.  Seznam všech restorable odstraněné databáze pomocí operace [seznam restorable zamítnuté databází][] .
2.  Seznamte se s podrobnostmi odstraněné databáze, kterou chcete obnovit pomocí operace [získat restorable zamítnuté databáze][] .
3.  Začněte myší [obnovení databáze vytvořit žádost o][] obnovení.
4.  Sledování stavu obnovení pomocí operace [stav operace databáze][] .

>[AZURE.NOTE] Po dokončení obnovení nastavení databáze naleznete v tématu [Konfigurace databáze po obnovení][]. 


## <a name="next-steps"></a>Další kroky
Další informace o funkcích business kontinuitu edicí databáze SQL Azure, přečtěte si [databáze SQL Azure obchodní kontinuitu přehled][].

<!--Image references-->

<!--Article references-->
[Základní informace kontinuitu firmy databáze SQL Azure]: ./sql-database-business-continuity.md
[Požadavek na změnu kvóty DTU]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Konfigurace databáze po obnovení]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How to install and configure Azure PowerShell]: ./powershell-install-configure.md
[Základní informace]: ./sql-data-warehouse-restore-database-overview.md
[Portál]: ./sql-data-warehouse-restore-database-portal.md
[Prostředí PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[ZBÝVAJÍCÍ]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->
[Vytvoření žádosti o obnovení databáze]: https://msdn.microsoft.com/library/azure/dn509571.aspx
[Stav operace databáze]: https://msdn.microsoft.com/library/azure/dn720371.aspx
[Získání restorable zamítnuté databáze]: https://msdn.microsoft.com/library/azure/dn509574.aspx
[Seznam restorable nezobrazí databází]: https://msdn.microsoft.com/library/azure/dn509562.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
