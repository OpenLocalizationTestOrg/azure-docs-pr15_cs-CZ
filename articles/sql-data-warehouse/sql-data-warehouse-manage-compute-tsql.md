<properties
   pageTitle="Správa výpočetního výkonu v Azure SQL datový sklad (REST) | Microsoft Azure"
   description="Skalární funkce (T-SQL) úkoly škálování výkonu úpravou DWUs. Uložení náklady roztažením zpět v čase a pozvolným."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/08/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-t-sql"></a>Správa výpočetního výkonu v Azure SQL datový sklad (T-SQL)

> [AZURE.SELECTOR]
- [Základní informace](sql-data-warehouse-manage-compute-overview.md)
- [Portál](sql-data-warehouse-manage-compute-portal.md)
- [Prostředí PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [ZBÝVAJÍCÍ](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Výkon měřítko tak, že rozšiřování vypočítat a materiály pro změnu požadavkům vaší pracovního vytížení paměti. Ušetřit náklady měřítka zpět zdrojů během není Špička nebo pozastavení výpočetním úplně odebrat. 

Tuto kolekci úkolech používá T-SQL tak, aby:

- Aktuální nastavení DWU zobrazení
- Zdroje pro využití změnit úpravou DWUs

Pozastavení nebo obnovení databáze, vyberte některou z dalších možností platformy v horní části tohoto článku.

Další informace najdete v tématu [Spravovat výpočet power – přehled][].

<a name="current-dwu-bk"></a>

## <a name="view-current-dwu-settings"></a>Aktuální nastavení DWU zobrazení

Pokud chcete zobrazit aktuální nastavení DWU pro databáze:

1. Otevřete Průzkumníka objektu SQL serveru ve Visual Studiu 2015.
2. Připojení k databázi předlohy přiřazené logické databáze SQL serveru.
2. Vyberte Správa dynamických zobrazení sys.database_service_objectives. Tady je příklad: 

```
SELECT
 db.name [Database],
 ds.edition [Edition],
 ds.service_objective [Service Objective]
FROM
 sys.database_service_objectives ds
 JOIN sys.databases db ON ds.database_id = db.database_id
```

<a name="scale-dwu-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a>Pro využití měřítko

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Chcete-li změnit DWUs:


1. Připojení k databázi předlohy přiřazené logické databáze SQL serveru.
2. Pomocí příkazu TSQL [Vlastnosti databáze][] . Následující příklad nastaví úrovně cíle služby DW1000 pro databázi MySQLDW. 

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Další kroky

Další úkoly správy najdete v článku [Přehled správy][].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Přehled správy]: ./sql-data-warehouse-overview-manage.md
[Správa výpočetním power – přehled]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->

[PŘÍKAZ ALTER DATABÁZE]: https://msdn.microsoft.com/library/mt204042.aspx


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
