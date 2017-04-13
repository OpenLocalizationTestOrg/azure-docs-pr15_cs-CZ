<properties
   pageTitle="Správa výpočetního výkonu v Azure SQL datový sklad (REST) | Microsoft Azure"
   description="Úkoly Powershellu ke správě výpočetního výkonu. Měřítko výpočet zdroje úpravou DWUs. Nebo pozastavení a obnovení výpočet nákladů na zdroje."
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

# <a name="manage-compute-power-in-azure-sql-data-warehouse-rest"></a>Správa výpočetního výkonu v Azure SQL datový sklad (REST)

> [AZURE.SELECTOR]
- [Základní informace](sql-data-warehouse-manage-compute-overview.md)
- [Portál](sql-data-warehouse-manage-compute-portal.md)
- [Prostředí PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [ZBÝVAJÍCÍ](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Výkon měřítko tak, že rozšiřování vypočítat a materiály pro změnu požadavkům vaší pracovního vytížení paměti. Ušetřit náklady měřítka zpět zdrojů během není Špička nebo pozastavení výpočetním úplně odebrat. 

Tuto kolekci úkolech používá Azure portálu:

- Pro využití měřítko
- Pozastavit výpočetním
- Pro využití životopisu

Další informace najdete v tématu [Správa výpočet přehled][].

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Měřítko výpočetního výkonu

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Pokud chcete změnit DWUs, pomocí rozhraní REST API [Vytvoření nebo aktualizace databáze][] . Následující příklad nastaví úrovně cíle služby DW1000 pro databázi MySQLDW, který je hostovaný na serveru Server. Server je ve skupině Azure zdroje s názvem ResourceGroup1.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/MyServer/databases/MySQLDW?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Pozastavit výpočetním

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Pozastavit databáze pomocí rozhraní REST API [Pozastavit databáze][] . Následující příklad pozastaví databázi s názvem Database02 hostovaných na serveru s názvem Server01. Server je ve skupině Azure zdroje s názvem ResourceGroup1.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/pause?api-version=2014-04-01-preview HTTP/1.1
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Pro využití životopisu

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Spusťte databáze pomocí rozhraní REST API [Životopisu databáze][] . Následující příklad spustí databázi s názvem Database02 hostovaných na serveru s názvem Server01. Server je ve skupině Azure zdroje s názvem ResourceGroup1. 

```
POST https://management.azure.com/subscriptions{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/resume?api-version=2014-04-01-preview HTTP/1.1
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Další kroky

Další úkoly správy najdete v článku [Přehled správy][].

<!--Image references-->

<!--Article references-->
[Přehled správy]: ./sql-data-warehouse-overview-manage.md
[Správa výpočetním přehled]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Pozastavit databáze]: https://msdn.microsoft.com/library/azure/mt718817.aspx
[Obnovení databáze]: https://msdn.microsoft.com/library/azure/mt718820.aspx
[Vytvoření nebo aktualizace databáze]: https://msdn.microsoft.com/library/azure/mt163685.aspx

<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
