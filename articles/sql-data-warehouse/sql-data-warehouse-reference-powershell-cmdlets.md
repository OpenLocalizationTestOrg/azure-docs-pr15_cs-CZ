<properties
   pageTitle="Rutiny prostředí PowerShell pro datový sklad SQL Azure"
   description="Najděte horní rutiny prostředí PowerShell pro datový sklad SQL Azure včetně postup pozastavit databáze."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="sonyama;barbkess;mausher"/>

# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a>Rutiny prostředí PowerShell a REST API pro datový sklad SQL

Mnoho úlohy správy SQL datový sklad můžete spravovat pomocí rutin prostředí PowerShell Azure nebo rozhraní REST API.  V následující tabulce jsou některé příklady použití příkazy Powershellu k automatizaci běžné úkoly v datový sklad SQL.  Příklady dobré ZBÝVAJÍCÍ najdete v článku [Správa škálovatelnost s ZBÝVAJÍCÍ][].

> [AZURE.NOTE]  Chcete-li pomocí prostředí PowerShell Azure SQL datový sklad, musíte Azure PowerShell verze 1.0.3 nebo vyšší.  Zkontrolujte verzi spuštěním **modul Get - ListAvailable – název Azure**.  Nejnovější verze lze nainstalovat z [Webové platformy Microsoft][].  Další informace o instalaci nejnovější verze najdete v článku [Jak nainstalovat a nakonfigurovat Azure Powershellu][].

## <a name="get-started-with-azure-powershell-cmdlets"></a>Začínáme s rutiny prostředí PowerShell Azure

1. Otevřete Windows PowerShell. 
2. Na příkazovém řádku prostředí PowerShell spusťte tyto příkazy k přihlášení správce prostředků Azure a vyberte předplatné.

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a>Příklad sklad dat pozastavit SQL

Zastavte ukazatel myši databázi s názvem "Database02" hostovaných na serveru s názvem "Server01."  Server je ve skupině Azure zdroje s názvem "ResourceGroup1." 

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
Změna, v tomto příkladu potrubí načtená objekt [Pozastavení AzureRmSqlDatabase][].  V důsledku toho se pozastaví databáze. Příkaz konečný zobrazuje výsledky.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a>Zahájení SQL datový sklad příklad

Obnovení databáze s názvem "Database02" hostovaných na serveru s názvem "Server01." Server je součástí skupiny zdroje s názvem "ResourceGroup1."

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Změna, v tomto příkladu načte databázi s názvem "Database02" ze serveru s názvem "Server01", který je součástí skupina zdroje s názvem "ResourceGroup1." Potrubí načtená objekt [AzureRmSqlDatabase životopisu][].

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
```

> [AZURE.NOTE] Všimněte si, že pokud váš server je foo.database.windows.net, použijte "foo" jako - název serveru v rutiny prostředí PowerShell.

## <a name="frequently-used-powershell-cmdlets"></a>Často rutiny prostředí PowerShell používá

Tyto rutiny prostředí PowerShell se často používá datový sklad SQL Azure.

- [Get-AzureRmSqlDatabase][]
- [Get-AzureRmSqlDeletedDatabaseBackup][]
- [Get-AzureRmSqlDatabaseRestorePoints][]
- [Nové AzureRmSqlDatabase][]
- [Odebrat AzureRmSqlDatabase][]
- [Obnovení AzureRmSqlDatabase][] 
- [Životopis AzureRmSqlDatabase][]
- [Vyberte AzureRmSubscription][]
- [Nastavení AzureRmSqlDatabase][]
- [Pozastavení AzureRmSqlDatabase][]

## <a name="next-steps"></a>Další kroky
Další příklady Powershellu najdete v článku:

- [Vytvořit datový sklad SQL pomocí prostředí PowerShell][]
- [Obnovení databáze][]

Seznam všech úkolů, které můžete automatické pomocí prostředí PowerShell najdete v tématu [Rutiny databáze SQL Azure][].  Seznam úkolů, které můžete automatické s ZBÝVAJÍCÍ najdete v tématu [operace pro databáze SQL Azure][].

<!--Image references-->

<!--Article references-->
[Instalace a konfigurace prostředí PowerShell Azure]: ./powershell-install-configure.md
[Vytvořit datový sklad SQL pomocí prostředí PowerShell]: ./sql-data-warehouse-get-started-provision-powershell.md
[Obnovení databáze]: ./sql-data-warehouse-restore-database-powershell.md
[Správa škálovatelnost s REST]: ./sql-data-warehouse-manage-compute-rest-api.md

<!--MSDN references-->
[Rutiny pro správu databáze Azure SQL]: https://msdn.microsoft.com/library/mt574084.aspx
[Operace pro databáze Azure SQL]: https://msdn.microsoft.com/library/azure/dn505719.aspx
[Get-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt603648.aspx
[Get-AzureRmSqlDeletedDatabaseBackup]: https://msdn.microsoft.com/library/mt693387.aspx
[Get-AzureRmSqlDatabaseRestorePoints]: https://msdn.microsoft.com/library/mt603642.aspx
[Nové AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Odebrat AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619368.aspx
[Obnovení AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx
[Životopis AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
<!-- It appears that Select-AzureRmSubscription isn't documented, so this points to Select-AzureSubscription -->
[Vyberte AzureRmSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Nastavení AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Pozastavení AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx

<!--Other Web references-->
[Microsoft Web platformy]: https://aka.ms/webpi-azps
