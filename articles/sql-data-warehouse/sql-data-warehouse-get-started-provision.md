<properties
   pageTitle="Vytvořit datový sklad SQL Azure portálu | Microsoft Azure"
   description="Naučte se vytvářet datový sklad SQL Azure na portálu Azure"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="jhubbard"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="barbkess;lodipalm;sonyama"/>

# <a name="create-an-azure-sql-data-warehouse"></a>Vytvořit datový sklad Azure SQL

> [AZURE.SELECTOR]
- [Azure portálu](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [Prostředí PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

Tento kurz slouží k vytváření datový sklad SQL, který obsahuje databáze ukázkové AdventureWorksDW portálu Azure.


## <a name="prerequisites"></a>Zjistit předpoklady pro

Začněte tím, budete potřebovat:

- **Účet Azure**: navštivte web [Bezplatnou zkušební verzi Azure][] nebo [MSDN Azure přeplatky][] vytvořte účet.
- **Server Azure SQL**: Další informace najdete v článku [vytvoření logické serveru databáze SQL Azure pomocí portálu Azure][] .

> [AZURE.NOTE] Vytváření SQL datový sklad může mít za následek novou fakturaci službu.  Další informace najdete v článku [SQL datový sklad ceny][] .

## <a name="create-a-sql-data-warehouse"></a>Vytvořit datový sklad SQL

1. Přihlaste se k [portálu Azure](https://portal.azure.com).

2. Klikněte na **+ Nový** > **dat + úložiště** > **SQL datový sklad**.

    ![Vytvoření](./media/sql-data-warehouse-get-started-provision/create-sample.gif)

3. V **SQL datový sklad** zásuvné vyplňte informace potřebné, stisknutím klávesy "Vytvořit".

    ![Vytvoření databáze](./media/sql-data-warehouse-get-started-provision/create-database.png)

    - **Server**: doporučujeme nejdřív vyberte server.  

    - **Název databáze**: název, který se používá jako odkaz datový sklad SQL.  Musí být jedinečný na server.
    
    - **Výkon**: doporučujeme počínaje 400 [DWUs][DWU]. Přesuňte jezdec doleva nebo doprava a upravte výkonu datový sklad nebo měřítko nahoru nebo dolů po vytvoření.  Další informace o DWUs, dokumentaci naše na [měřítka](./sql-data-warehouse-manage-compute-overview.md) nebo naše [ceny stránky][SQL datový sklad ceny]. 

    - **Předplatné**: vyberte [předplatné] , který k vám bude účtovat tento datový sklad SQL.

    - **Pole Skupina zdroje**: [skupiny zdrojů] [ Resource group] jsou kontejnery navržen tak, aby vám pomůžou se správou kolekce Azure prostředků. Další informace o [skupinách zdroje](../azure-resource-manager/resource-group-overview.md).

    - **Vyberte zdroj**: klikněte na **Vybrat zdroj** > **vzorku**. Azure automaticky naplní možnost **Vybrat vzorek** s AdventureWorksDW.

> [AZURE.NOTE] Výchozí řazení SQL datový sklad je SQL_Latin1_General_CP1_CI_AS. V případě potřeby různé řazení [T-SQL][] mohou sloužit k vytvoření databáze pomocí různých řazení.

4. Klikněte na **vytvořit** vytvořit datový sklad SQL.

5. Počkejte několik minut. Pokud datový sklad hotovou, má být vrácena [Azure portálu](https://portal.azure.com). Datový sklad SQL můžete najít na řídicím panelu, uvedené v části SQL databáze nebo ve skupině zdroje, která jste použili k jeho vytvoření. 

    ![zobrazení portálu](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[AZURE.INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)] 

## <a name="next-steps"></a>Další kroky

Teď, když jste vytvořili datový sklad SQL, budete chtít [Připojit](./sql-data-warehouse-connect-overview.md) a začněte dotazu.

Načíst data do SQL datový sklad, najdete v článku [načítání přehled](./sql-data-warehouse-overview-load.md).

Pokud chcete migrovat stávající databázi SQL datový sklad, najdete v článku [Přehled migrace](./sql-data-warehouse-overview-migrate.md) nebo použití [Nástroje pro migraci](./sql-data-warehouse-migrate-migration-utility.md).

Pravidla brány firewall lze také pomocí nakonfigurovat jazyce Transact-SQL. Další informace najdete v tématu [sp_set_firewall_rule][] a [sp_set_database_firewall_rule][].

Je taky skvělý nápad podívat se na [osvědčené postupy][].

<!--Article references-->
[Vytvoření logické serveru databáze SQL Azure pomocí portálu Azure]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Create an Azure SQL Database logical server with PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[resource groups]: ../resource-group-template-deploy-portal.md
[Doporučené postupy]: sql-data-warehouse-best-practices.md
[DWU]: sql-data-warehouse-overview-what-is.md#data-warehouse-units
[předplatné]: ../azure-glossary-cloud-terminology.md#subscription
[resource group]: ../azure-glossary-cloud-terminology.md#resource-group
[T-SQL]: ./sql-data-warehouse-get-started-create-database-tsql.md
 
<!--MSDN references-->
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx

<!--Other Web references-->
[SQL datový sklad ceny]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Bezplatnou zkušební verzi Azure]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure kreditů]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F

