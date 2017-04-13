<properties
    pageTitle="Obnovení jednu tabulku ze zálohy databáze SQL Azure | Microsoft Azure"
    description="Zjistěte, jak obnovit jednu tabulku ze zálohy databáze SQL Azure."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="daleche"/>


# <a name="how-to-restore-a-single-table-from-an-azure-sql-database-backup"></a>Jak obnovit jednu tabulku ze zálohy databáze SQL Azure

Může nastat situace, ve kterém jste omylem upravili některá data do SQL databáze a teď chcete obnovit jednoho problémového tabulku. Tento článek popisuje, jak obnovit jedné tabulky v databázi z jednoho z databáze SQL [automatického zálohování](sql-database-automated-backups.md).

## <a name="preparation-steps-rename-the-table-and-restore-a-copy-of-the-database"></a>Přípravných kroků: Přejmenování tabulky a obnovení kopii databáze
1. Identifikujte tabulku v databázi Azure SQL, který chcete nahradit obnovená kopií. Pomocí aplikace Microsoft SQL Management Studio přejmenuje tabulku. Například přejmenování tabulky jako &lt;název tabulky&gt;_old.

    **Poznámka:** Chcete-li předejít blokovány, ujistěte se, že je neaktivní provozu v tabulce, který chcete přejmenovat. Pokud narazíte na problémy, ujistěte se, pomocí tohoto postupu době údržby.

2. Obnovení záložní databáze bod, který chcete obnovit pomocí kroků [Čárky In_Time obnovit](sql-database-recovery-using-backups.md#point-in-time-restore) .

    **Poznámky**:
    - Název obnovenou databázi bude ve formátu DBName + časové razítko. například **Adventureworks2012_2016 01 01T22 12Z**. Tento krok nepřepíše stávající název databáze na serveru. Toto je míra zabezpečení a má určené umožňuje před přetáhnout jejich aktuální databáze a přejmenujte obnovené databáze pro použití v praxi ověřit obnovenou databázi.
    - Všechny úrovně výkonu z základní Premium automaticky zálohují službou s různou metriky záložní uchovávání informací, v závislosti na úrovni:

| Obnovení databáze | Základní vrstvy | Standardní úrovní | Úrovně Premium |
| :-- | :-- | :-- | :-- |
|  Kdykoli obnovit |  Některé bod obnovení do 7 dnů|Některé bod obnovení do 35 dnů| Některé bod obnovení do 35 dnů|

## <a name="copying-the-table-from-the-restored-database-by-using-the-sql-database-migration-tool"></a>Zkopírujte tabulku z obnovení databáze pomocí nástroje pro migraci databáze SQL
1. Stažení a instalace [Průvodce přenesením databáze SQL](https://sqlazuremw.codeplex.com).

2. Otevřít Průvodce migrací databáze SQL na stránce **Vyberte proces** , vyberte **databázi v části analyzovat/migrace**a klikněte na tlačítko **Další**.
![Migrace databáze SQL wizard – vyberte proces](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/1.png)
3. V dialogovém okně **připojit k serveru** použijte následující nastavení:
 - **Název serveru**: instance Your SQL Azure
 - **Ověření**: **ověřování serveru SQL Server**. Zadejte svoje přihlašovací údaje.
 - **Databáze**: **předlohy DB (seznam všechny databáze)**.
 - **Poznámka:** Ve výchozím nastavení uloží vaše přihlašovací údaje. Pokud nechcete, aby ji, vyberte **v žádném případě nezapomeňte přihlašovací údaje**.
![Migrace databáze SQL kroku průvodce – vyberte zdroj - 1](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/2.png)
4. V dialogovém okně **Vybrat zdroj** vyberte název obnovená databáze z oddílu **přípravných kroků** jako zdroj a potom klikněte na **Další**.

    ![Migrace databáze SQL průvodce – vyberte zdroj - krok 2](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/3.png)

5. V dialogovém okně **Vybrat objekty** vyberte možnost **vybrat konkrétní databázových objektů** a vyberte table(or tables), kterou chcete migrovat do cílového serveru.
![Migrace databáze SQL wizard – zvolte objekty](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/4.png)

6. Na stránce **Souhrn skript Průvodce** kliknutím na **Ano** po zobrazení výzvy o tom, jestli jste připraveni generovat skript SQL. Máte taky možnost uložit TSQL skriptu pro pozdější použití.
![Migrace databáze SQL wizard – skript Průvodce souhrn](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/5.png)

7. Na stránce **Souhrn výsledků** klikněte na **Další**.
![Migrace databáze SQL wizard – souhrn výsledků](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/6.png)

8. Na stránce **Nastavení cílové serveru připojení** klikněte na **připojit k serveru**a zadejte podrobnosti následujícím způsobem:
    - **Název serveru**: cílové instance serveru
    - **Ověření**: **ověřování serveru SQL Server**. Zadejte svoje přihlašovací údaje.
    - **Databáze**: **předlohy DB (seznam všechny databáze)**. Tuto možnost uvádí všechny databáze na cílovém serveru.

    ![Migrace databáze SQL wizard – nastavení cílové serveru připojení](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/7.png)

9. Klikněte na **Připojit**, vyberte cílovou databázi, kterou chcete přesunout do tabulky a klikněte na tlačítko **Další**. To měli dokončení spustit dříve vytvořený skript a byste měli vidět tabulce nově přesunutý zkopírují do cílové databáze.

## <a name="verification-step"></a>Krok ověření
1. Dotazy a otestujte nově kopírované tabulky a ujistěte se, že data je funkční. Po potvrzení můžete přetáhnout formuláře přejmenované tabulky oddíl **přípravných kroků** . (například &lt;název tabulky&gt;_old).

## <a name="next-steps"></a>Další kroky

[Automatické zálohování databáze SQL](sql-database-automated-backups.md)
