<properties 
    pageTitle="Konfigurace Geo replikace databáze SQL Azure pomocí portálu Azure | Microsoft Azure" 
    description="Konfigurace Geo replikace databáze SQL Azure pomocí portálu Azure" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="NA"
    ms.date="10/18/2016"
    ms.author="sstein"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-the-azure-portal"></a>Konfigurace Geo replikace databáze SQL Azure pomocí portálu Azure


> [AZURE.SELECTOR]
- [Základní informace](sql-database-geo-replication-overview.md)
- [Azure portálu](sql-database-geo-replication-portal.md)
- [Prostředí PowerShell](sql-database-geo-replication-powershell.md)
- [T-SQL](sql-database-geo-replication-transact-sql.md)

Tento článek popisuje, jak nakonfigurovat aktivní Geo replikace databáze SQL s [Azure portálu](http://portal.azure.com).

Zahajte převzetí pomocí portálu Azure, najdete v článku [Zahájit plánované nebo neplánované selhání databáze SQL Azure pomocí portálu Azure](sql-database-geo-replication-failover-portal.md).

>[AZURE.NOTE] Aktivní Geo replikace (čitelné druhotné) je teď k dispozici pro všechny databáze ve všech vrstvách služby. V duben 2017-čitelné sekundární typ bude vyřadit a existující-čitelné databáze upgradují automaticky k čitelné druhotné.

Abyste mohli nakonfigurovat Geo replikace pomocí portálu Azure, musíte následující zdroje:

- Azure SQL databázi - primární databáze, kterou chcete replikovat na jinou zeměpisnou oblast.

## <a name="add-secondary-database"></a>Přidání sekundární databáze

Podle těchto kroků vytvořit novou databázi sekundární Geo replikace partnerství.  

Přidat sekundární, musíte být vlastník předplatného nebo spoluvlastníka. 

Sekundární databáze bude mít stejný název jako primární databáze a ve výchozím nastavení mají stejné úrovni služby. Sekundární databáze může být jednu databázi nebo pružná databázi. Další informace najdete v tématu [– Úrovně služeb](sql-database-service-tiers.md).
Po vytvoření a nasadí sekundární začne se data replikace z primární databáze do nové sekundární databáze. 

> [AZURE.NOTE] Pokud již existuje databáze partnera (například - důsledku ukončení předchozí relace Geo replikace) na příkaz se nezdaří.

### <a name="add-secondary"></a>Přidání sekundární

1. [Azure portál](http://portal.azure.com)přejděte k databázi, kterou chcete nastavit Geo replikace.
2. Na stránce SQL databáze zaškrtněte políčko **Geo replikace**a vyberte oblasti, kterou chcete vytvořit sekundární databázi. Můžete vybrat všechny oblasti než oblasti hostingu primární databáze, [párových oblast](../best-practices-availability-paired-regions.md) doporučuje se ale.

    ![Konfigurace Geo replikace](./media/sql-database-geo-replication-portal/configure-geo-replication.png)


4. Vyberte nebo konfigurace serveru a ceny osy sekundární databáze.

    ![Konfigurace sekundárního](./media/sql-database-geo-replication-portal/create-secondary.png)

5. Pokud chcete můžete přidat sekundární databáze fondu pružná databáze:

 Vytvoření sekundární databáze do fondu, klikněte na **fondu pružná databáze** a vyberte fond na cílovém serveru. Fond, musí existovat na cílovém serveru jako tento pracovní postup nelze vytvořit skupinu.

6. Klikněte na **vytvořit** přidáte sekundární.
 
6. Sekundární databáze se vytvoří a osazení proces, nebude zahájen. 
 
    ![Konfigurace sekundárního](./media/sql-database-geo-replication-portal/seeding0.png)

7. Po dokončení procesu osazení sekundární databáze zobrazí její stav.

    ![ohlašovat dokončení](./media/sql-database-geo-replication-portal/seeding-complete.png)


## <a name="remove-secondary-database"></a>Odebrat sekundární databázi

Operace trvale ukončí replikace sekundární databáze a rolí sekundární se změní na standardní databázi pro čtení i zápis. Při přerušení připojení k databázi sekundární příkazu povede, ale sekundární bude nemohly pro čtení i zápis až po připojení se obnoví.  

1. V [Azure portál](http://portal.azure.com)přejděte k primární databázi partnerství Geo replikace.
2. Na stránce SQL databáze zaškrtněte políčko **Geo replikace**.
3. V seznamu **druhotné** vyberte databázi, kterou chcete odebrat z partnerství Geo replikace.
4. Klepněte na tlačítko **Zastavit replikace**.

    ![Odebrat sekundární](./media/sql-database-geo-replication-portal/remove-secondary.png)

5. Klepnutím na **Ukončit replikace** otevře okno s potvrzením tak kliknutím na tlačítko **Ano** odebrání partnerství Geo replikace databáze (nastavte není součástí všech replikace databáze pro čtení i zápis).


## <a name="next-steps"></a>Další kroky

- Další informace o aktivní Geo replikace najdete v tématu - [Aktivní Geo replikace](sql-database-geo-replication-overview.md)
- Přehled kontinuitu business a scénáře najdete v tématu [Přehled kontinuitu Business](sql-database-business-continuity.md)

