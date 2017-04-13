<properties
   pageTitle="Vytvořit datový sklad SQL pomocí prostředí PowerShell | Microsoft Azure"
   description="Vytvořit datový sklad SQL pomocí prostředí PowerShell"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="create-sql-data-warehouse-using-powershell"></a>Vytvořit datový sklad SQL pomocí prostředí PowerShell

> [AZURE.SELECTOR]
- [Azure portálu](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [Prostředí PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

Tento článek popisuje, jak vytvořit datový sklad SQL pomocí Powershellu.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Začněte tím, budete potřebovat:

- **Účet Azure**: navštivte web [Bezplatnou zkušební verzi Azure][] nebo [MSDN Azure přeplatky][] vytvořte účet.
- **Server Azure SQL**: Podrobnosti najdete v části [vytvořit logické server databáze SQL Azure pomocí portálu Azure][] nebo [vytvořit logické server databáze SQL Azure pomocí prostředí PowerShell][] Další.
- **Pole Skupina zdroje**: používají stejné skupiny prostředků serveru Azure SQL nebo najdete v článku [jak vytvářet skupiny zdrojů][].
- **Prostředí PowerShell verze 1.0.3 nebo vyšší**: Zkontrolujte verzi spuštěním **modul Get - ListAvailable – název Azure**.  Nejnovější verze lze nainstalovat z [Webové platformy Microsoft][].  Další informace o instalaci nejnovější verze najdete v článku [Jak nainstalovat a nakonfigurovat Azure Powershellu][].

> [AZURE.NOTE] Vytváření datový sklad SQL může mít za následek novou fakturaci službu.  Další informace o cenách najdete v článku [SQL datový sklad ceny][] .

## <a name="create-a-sql-data-warehouse"></a>Vytvořit datový sklad SQL

1. Otevřete Windows PowerShell.
2. Spusťte tuto rutinu k přihlášení k správce prostředků Azure.

    ```Powershell
    Login-AzureRmAccount
    ```
    
3. Vyberte předplatné, které chcete použít pro aktuální relaci.

    ```Powershell
    Get-AzureRmSubscription -SubscriptionName "MySubscription" | Select-AzureRmSubscription
    ```

4.  Vytvoření databáze. Tento příklad vytvoří databázi s názvem "mynewsqldw" s cíle úrovně služeb "DW400" server s názvem "sqldwserver1", která je ve skupině zdroje s názvem "mywesteuroperesgp1".

    ```Powershell
    New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse" -CollationName "SQL_Latin1_General_CP1_CI_AS" -MaxSizeBytes 10995116277760
    ```

Požadované parametry jsou:

- **RequestedServiceObjectiveName**: množství [DWU][] požadujete.  Podporované hodnoty jsou: DW100 DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000 a DW6000.
- **Název databáze**: název datový sklad SQL, který vytváříte.
- **Webu název serveru**: název serveru, který používáte pro vytváření (musí být V12).
- **ResourceGroupName**: používáte pole Skupina zdroje.  Při hledání zdrojů k dispozici použijte skupiny ve vašem předplatném Get-AzureResource.
- **Edition**: musí být "DataWarehouse" vytvořit datový sklad SQL.

Volitelné parametry jsou:

- **CollationName**: výchozí řazení, pokud není zadán je SQL_Latin1_General_CP1_CI_AS.  Řazení nelze změnit na databázi.
- **MaxSizeBytes**: výchozí maximální velikost databáze je 10 GB.


Podrobné informace o možnosti parametrů najdete v článku [Nový AzureRmSqlDatabase][] a [Vytvořit databázi (Azure SQL datový sklad)][].

## <a name="next-steps"></a>Další kroky

Po dokončení SQL datový sklad zřizování chtít zopakujte [načtení ukázkových dat][] nebo najdete v článku Jak [vývoj][], [načtení][]nebo [migrace][].

Pokud vás zajímají další informace o tom, jak spravovat SQL datový sklad programově, přečtěte si náš článku o používání [rutin prostředí PowerShell a rozhraní REST API][].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[migrace]: ./sql-data-warehouse-overview-migrate.md
[Můžete vyvíjet]: ./sql-data-warehouse-overview-develop.md
[načtení]: ./sql-data-warehouse-load-with-bcp.md
[načtení ukázkových dat]: ./sql-data-warehouse-load-sample-databases.md
[Rutiny prostředí PowerShell a rozhraní REST API]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[firewall rules]: ../sql-database-configure-firewall-settings.md

[Instalace a konfigurace prostředí PowerShell Azure]: ../powershell/powershell-install-configure.md
[how to create a SQL Data Warehouse from the Azure Portal]: ./sql-data-warehouse-get-started-provision.md
[Vytvoření logické serveru databáze SQL Azure pomocí portálu Azure]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Vytvoření logické serveru databáze SQL Azure pomocí prostředí PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[jak vytvořit skupinu zdrojů]: ../resource-group-template-deploy-portal.md#create-resource-group

<!--MSDN references--> 
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[Nové AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Vytvoření databáze (Azure SQL datový sklad)]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[Microsoft Web platformy]: https://aka.ms/webpi-azps
[SQL datový sklad ceny]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Bezplatnou zkušební verzi Azure]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure kreditů]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
