
<!--
includes/sql-database-create-new-database-portal.md

Latest Freshness check:  2016-04-11 , carlrab.

As of circa 2016-04-11, the following topics might include this include:
articles/sql-database/sql-database-get-started-tutorial.md

-->
## <a name="create-a-new-azure-sql-database"></a>Vytvořit novou databázi Azure SQL

Pomocí následujících kroků v portálu Azure na nové nebo existující databáze SQL Azure logické serveru vytvořit novou databázi Azure SQL.

1. Pokud jste připojení nesynchronizujete, připojte k [Azure portálu](http://portal.azure.com).
2. Klikněte na **Nový**, zadejte **SQL databáze**a klikněte na **Databáze SQL (nové databáze)**.

     ![Nové databáze](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-1.png)

3. Klikněte na **Databáze SQL (nové databáze)**.

     ![Nové databáze](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-2.png)

4. Kliknutím na tlačítko **vytvořit** vytvořte novou databázi služby SQL databáze.

     ![Nové databáze](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-3.png)

5. Obsahují hodnoty pro následující vlastnosti serveru:

 - Název databáze
 - Předplatné: Toto nastavení se projeví jenom v případě, že máte víc předplatných.
 - Pole Skupina zdroje: Pokud právě začínáte, použijte skupina zdroje logické serveru.
 - Vyberte zdroj: můžete vybrat prázdnou databázi, ukázková data nebo zálohování Azure databáze. Migrovat databázi místního serveru SQL Server nebo načtení dat pomocí nástroje příkazového řádku BCP, zobrazíte pomocí odkazů na konci tohoto článku.
 - Serveru: Nové nebo existující logické serveru.
 - Přihlášení k správce serveru
 - Heslo
 - Ceny osy: Pokud právě začínáte, použijte výchozí hodnotu S0.
 - Řazení: Toto nastavení se projeví jenom v případě, že byla vybrána prázdná databáze.

        ![New database](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-4.png)

6.  Klikněte na **vytvořit**. V oznamovací oblasti uvidíte, že vaše nasazení spustila.

     ![Nové databáze](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-5.png)

7. Počkejte nasazení dokončit před pokračováním k dalšímu kroku.

     ![Nové databáze](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-6.png)
