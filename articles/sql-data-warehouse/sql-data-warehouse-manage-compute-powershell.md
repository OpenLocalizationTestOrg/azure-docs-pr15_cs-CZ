<properties
   pageTitle="Správa výpočetního výkonu v Azure SQL datový sklad (Powershellu) | Microsoft Azure"
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
   ms.date="06/13/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-powershell"></a>Správa výpočetního výkonu v Azure SQL datový sklad (Powershellu)

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


## <a name="before-you-begin"></a>Než začnete

### <a name="install-the-latest-version-of-azure-powershell"></a>Nainstalujte nejnovější verzi Azure PowerShell

> [AZURE.NOTE]  Prostředí PowerShell Azure pomocí služby SQL datový sklad, musíte Azure PowerShell verze 1.0.3 nebo vyšší.  Pro ověření vaší aktuální verze spuštění příkazu **modul Get - ListAvailable – název Azure**. Z [Microsoft webové platformy][]můžete nainstalovat nejnovější verzi.  Další informace najdete v tématu [instalace a konfigurace prostředí PowerShell Azure][].

### <a name="get-started-with-azure-powershell-cmdlets"></a>Začínáme s rutiny prostředí PowerShell Azure

Jak začít:

1. Otevřete Azure Powershellu. 
2. Na příkazovém řádku prostředí PowerShell spusťte tyto příkazy k přihlášení správce prostředků Azure a vyberte předplatné.

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Měřítko výpočetního výkonu

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Pokud chcete změnit DWUs, pomocí rutiny prostředí PowerShell [Set-AzureRmSqlDatabase][] . Následující příklad nastaví úrovně cíle služby DW1000 pro databázi MySQLDW, který je hostovaný na serveru Server. 

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Pozastavit výpočetním

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Pozastavit databáze, získáte pomocí rutiny [Pozastavení AzureRmSqlDatabase][] . Následující příklad pozastaví databázi s názvem Database02 hostovaných na serveru s názvem Server01. Server je ve skupině Azure zdroje s názvem ResourceGroup1. 

> [AZURE.NOTE] Všimněte si, že pokud váš server je foo.database.windows.net, použijte "foo" jako - název serveru v rutiny prostředí PowerShell.

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
```
Některou variantu dalším příkladu načte databáze do $database objektu. Potom potrubí objekt [Pozastavení AzureRmSqlDatabase][]. Výsledky jsou uloženy v resultDatabase objektu. Příkaz konečný zobrazuje výsledky.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Pro využití životopisu

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Spuštění databáze, získáte pomocí rutiny [AzureRmSqlDatabase životopisu][] . Následující příklad spustí databázi s názvem Database02 hostovaných na serveru s názvem Server01. Server je ve skupině Azure zdroje s názvem ResourceGroup1. 

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" -DatabaseName "Database02"
```

Některou variantu dalším příkladu načte databáze do $database objektu. Potom potrubí objekt [Životopisu AzureRmSqlDatabase][] a výsledek uloží do $resultDatabase. Příkaz konečný zobrazuje výsledky.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Další kroky

Další úkoly správy najdete v článku [Přehled správy][].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Přehled správy]: ./sql-data-warehouse-overview-manage.md
[Instalace a konfigurace prostředí PowerShell Azure]: ./powershell-install-configure.md
[Správa výpočetním přehled]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Životopis AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
[Pozastavení AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx
[Nastavení AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx

<!--Other Web references-->
[Microsoft Web platformy]: https://aka.ms/webpi-azps
[Azure portal]: http://portal.azure.com/
