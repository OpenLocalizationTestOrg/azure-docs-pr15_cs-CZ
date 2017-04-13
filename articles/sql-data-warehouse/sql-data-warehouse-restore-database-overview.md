<properties
   pageTitle="Obnovení SQL datový sklad | Microsoft Azure"
   description="Základní informace o možnostech obnovení databáze pro obnovení databáze v Azure SQL datový sklad."
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
   ms.date="09/29/2016"
   ms.author="lakshmir;barbkess;sonyama"/>


# <a name="sql-data-warehouse-restore"></a>Obnovení SQL datový sklad

> [AZURE.SELECTOR]
- [Základní informace][]
- [Portál][]
- [Prostředí PowerShell][]
- [ZBÝVAJÍCÍ][]

SQL datový sklad nabízí místní a zeměpisné obnoví v rámci jeho datový sklad katastrofě možnosti obnovení. Použít datový sklad zálohy obnovíte datový sklad bod obnovení v oblasti hlavní nebo geo nadbytečné zálohy použít k obnovení pro jinou zeměpisnou oblast. Tento článek vysvětluje konkrétní obnovení datový sklad.

## <a name="what-is-a-data-warehouse-restore"></a>Co je datový sklad obnovení?

Obnovení dat sklad je nový datový sklad vytvořený ze zálohy existující nebo odstraněné datový sklad. Obnovené datový sklad znovu vytvoří zálohované datový sklad v daném čase. Protože SQL datový sklad distribuovaného systému, vytvoří se sklad obnovení dat z mnoha záložních souborů, které jsou uložené v objektů BLOB Azure. 

Obnovení databáze je důležitou součástí všech obchodní strategie pro obnovení kontinuitu a havárie, protože znovu vytvoří data po náhodné poškození nebo odstranění.

Další informace najdete v tématu:

-  [Zálohování SQL datový sklad](sql-data-warehouse-backups.md)
-  [Obchodní kontinuitu přehled](../sql-database/sql-database-business-continuity.md)

## <a name="data-warehouse-restore-points"></a>Datový sklad obnovení body

Jako výhodou používání úložišti Premium Azure SQL datový sklad používá Azure úložiště objektů Blob snímky zálohování primární datový sklad. Každý snímek obsahuje bod, který představuje při spuštění snímek. Obnovení datový sklad, zvolte bod obnovení a příkaz Obnovit.  

SQL datový sklad vždy obnoví zálohování nový datový sklad. Můžete ponechat obnovená datový sklad a aktuálním řádkem, nebo odstranit jeden z nich. Pokud chcete nahradit aktuální datový sklad obnovená datový sklad, můžete ji přejmenovat.

Pokud budete muset obnovit odstraněné nebo pozastaveném datový sklad, můžete [vytvořit lístek podpory](sql-data-warehouse-get-started-create-support-ticket.md). 

<!-- 
### Can I restore a deleted data warehouse?

Yes, you can restore the last available restore point.

Yes, for the next seven calendar days. When you delete a data warehouse, SQL Data Warehouse actually keeps the data warehouse and its snapshots for seven days just in case you need the data. After seven days, you won't be able to restore to any of the restore points. -->

## <a name="geo-redundant-restore"></a>GEO nadbytečné obnovení

Pokud používáte geo nadbytečné úložiště, můžete obnovit datový sklad [párových datacentrem](../best-practices-availability-paired-regions.md) na jinou zeměpisnou oblast. Datový sklad obnovit z posledního denní zálohování. 

## <a name="restore-timeline"></a>Obnovení časové osy

Libovolný bod k dispozici obnovení můžete obnovit databázi během posledních sedmi dnů. Snímky spusťte každých čtyři až osm hodin a jsou dostupné pro sedmi dnů. Je-li snímek starší než 7 dnů, vypršením jeho platnosti a jeho obnovení bod už není dostupná.

## <a name="restore-costs"></a>Obnovení nákladů

Využití úložiště obnovená datový sklad fakturované sazbou Azure Premium úložiště. 

Zastavte ukazatel myši obnovená datový sklad vám bude účtovaná za úložiště sazbou Azure Premium úložiště. Má pozastavení tu výhodu, že nejsou účtovaná za DWU výpočetních prostředků.

Další informace o SQL datový sklad ceny najdete v článku [SQL datový sklad ceny](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="uses-for-restore"></a>Použití obnovení

Primární použití datový sklad obnovení je obnovit data po náhodné ztrátě nebo poškození.

Můžete také datový sklad obnovení uchovávání zálohy po dobu delší než sedmi dnů. Po obnovení zálohy jste datový sklad online a můžete ho donekonečna udržovat výpočetním snížení nákladů na pozastavit. Pozastavené databáze poněkud úložiště poplatky sazbou Azure Premium úložiště. 

## <a name="related-topics"></a>Příbuzná témata

### <a name="scenarios"></a>Scénáře

- Přehled kontinuitu firmy najdete v článku [Přehled kontinuitu Business](../sql-database/sql-database-business-continuity.md)


<!-- ### Tasks -->

Pro obnovení datový sklad, obnovte pomocí:

- Azure portálem, najdete v článku [obnovení datový sklad pomocí portálu Azure](sql-data-warehouse-restore-database-portal.md)
- Rutiny prostředí PowerShell najdete v článku [obnovení datový sklad pomocí rutin prostředí PowerShell](sql-data-warehouse-restore-database-powershell.md)
- Když POSUNETE rozhraní API najdete v článku [obnovení datový sklad pomocí rozhraní REST API](sql-data-warehouse-restore-database-rest-api.md)

<!-- ### Tutorials -->

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Základní informace]: ./sql-data-warehouse-restore-database-overview.md
[Portál]: ./sql-data-warehouse-restore-database-portal.md
[Prostředí PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[ZBÝVAJÍCÍ]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->


<!--Other Web references-->
