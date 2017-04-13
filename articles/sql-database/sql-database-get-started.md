<properties
    pageTitle="Databáze SQL kurz: vytvoření databáze SQL | Microsoft Azure"
    description="Zjistěte, jak nastavit logické databáze SQL serveru, serveru pravidlo brány firewall, databáze SQL a ukázková data. Také Naučte se spojit s klientských nástrojích konfigurace uživatelů a nastavení brány firewall pravidla databáze."
    keywords="SQL databáze kurz vytvoření databáze sql"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/07/2016"
    ms.author="carlrab"/>


# <a name="sql-database-tutorial-create-a-sql-database-in-minutes-by-using-the-azure-portal"></a>Databáze SQL kurz: vytvoření databáze SQL v minutách pomocí portálu Azure

> [AZURE.SELECTOR]
- [Azure portálu](sql-database-get-started.md)
- [C#](sql-database-get-started-csharp.md)
- [Prostředí PowerShell](sql-database-get-started-powershell.md)

V tomto kurzu se naučíte, jak používat portál Azure na:

- Vytvoření databáze Azure SQL s ukázkovými daty.
- Vytvořte pravidlo brány firewall úrovni serveru pro jednu IP adresu nebo pro rozsah IP adres.

Provedení těchto úloh pomocí [C#](sql-database-get-started-csharp.md) nebo pomocí [Powershellu](sql-database-get-started-powershell.md).

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]

<a name="create-logical-server-bk"></a>

## <a name="create-your-first-azure-sql-database"></a>Vytvoření první databáze Azure SQL 

1. Pokud jste připojení nesynchronizujete, připojte k [Azure portálu](http://portal.azure.com).
2. Klikněte na **Nový**, klikněte na **Data + úložiště**a vyhledejte **Databáze SQL**.

    ![Nové databáze sql 1](./media/sql-database-get-started/sql-database-new-database-1.png)

3. Klikněte na **Databáze SQL** otevřete zásuvné databáze SQL. Obsah na tomto zásuvné se liší v závislosti na počtu předplatné a existující objekty (například stávající servery).

    ![Nové databáze sql 2](./media/sql-database-get-started/sql-database-new-database-2.png)

4. Do pole **název databáze** zadejte název pro první databáze – například "Moje databáze". Zelená značka zaškrtnutí označuje, že jste zadali platný název.

    ![Nové databáze sql 3](./media/sql-database-get-started/sql-database-new-database-3.png)

5. Pokud máte víc předplatných vyberte předplatné.
6. V části **Skupina zdroje**klikněte na **vytvořit nový** a zadejte název pro první skupina zdroje – například "my-– skupina zdroje". Zelená značka zaškrtnutí označuje, že jste zadali platný název.

    ![Nové databáze sql 4](./media/sql-database-get-started/sql-database-new-database-4.png)

7. Ve skupinovém rámečku **Vyberte zdroj výsledků**klikněte na **vzorek** a klikněte v seznamu **Vyberte ukázkový** **AdventureWorksLT [V12]**.

    ![Nové databáze sql 5](./media/sql-database-get-started/sql-database-new-database-5.png)

8. V části **Server**klikněte na **konfigurovat požadované nastavení**.

    ![Nové databáze sql 6](./media/sql-database-get-started/sql-database-new-database-6.png)

9. Na zásuvné serveru klikněte na **vytvořit nový server**. V rámci objektu serveru, který může být serveru nového nebo existujícího serveru se vytvoří databázi Azure SQL.

    ![Nové databáze sql 7](./media/sql-database-get-started/sql-database-new-database-7.png)

10. Prohlédněte si **Nový server** zásuvné pochopit informace, které budete muset zadat nové serveru.

    ![Nové databáze sql 8](./media/sql-database-get-started/sql-database-new-database-8.png)

11. Do pole **název serveru** zadejte název serveru první – například "my nový server objekt". Zelená značka zaškrtnutí označuje, že jste zadali platný název.

    ![Nové databáze sql 9](./media/sql-database-get-started/sql-database-new-database-9.png)
 
12. V části **přihlašovací správce serveru**zadejte uživatelské jméno pro přihlášení správce pro tento server – například "my--účtu správce". Toto přihlášení se označuje jako hlavní přihlašovací server. Zelená značka zaškrtnutí označuje, že jste zadali platný název.

    ![Nové databáze sql 10](./media/sql-database-get-started/sql-database-new-database-10.png)

13. V části **heslo** a **potvrďte heslo**zadejte heslo pro server hlavní přihlašovací účet – jako "p@ssw0rd1". Zelená značka zaškrtnutí označuje, že jste zadali platné heslo.

    ![Nové databáze sql 11](./media/sql-database-get-started/sql-database-new-database-11.png)
 
14. V části **umístění**vyberte příslušnou k vaší poloze – například "Austrálie východ" datovém centru.

    ![Nové databáze sql 12](./media/sql-database-get-started/sql-database-new-database-12.png)

15. V části ** Všimněte si vytvořit V12 serveru (nejnovější aktualizace), stačí jenom možnost vytvořit aktuální verzi Azure SQL serveru.

    ![Nové databáze sql 13](./media/sql-database-get-started/sql-database-new-database-13.png)

16. Všimněte si, že ve výchozím nastavení zaškrtávací políčko **Povolit azure** služeb pro přístup k serveru zaškrtnuté a není možné měnit. Toto je Upřesnit možnosti. V nastavení serveru brány firewall pro tento objekt serveru toto nastavení můžete změnit pro většinu scénářů není to však nutné.

    ![Nové databáze sql 14](./media/sql-database-get-started/sql-database-new-database-14.png)

17. Na nové zásuvné serveru zkontrolujte vybrané položky a klikněte na možnost **Vybrat** vyberte tohoto serveru pro novou databázi.

    ![Nové databáze sql 15](./media/sql-database-get-started/sql-database-new-database-15.png)

18. Na zásuvné SQL databáze ve skupinovém rámečku **ceny osy**klikněte na **Standardní S2** a klikněte na **základní** zvolit nejméně drahé ceny osy první databáze. Ceny osy můžete kdykoli změnit později.

    ![Nové databáze sql 16](./media/sql-database-get-started/sql-database-new-database-16.png)

19. Na zásuvné databáze SQL zkontrolujte vybrané položky a potom kliknutím na **vytvořit** vytvoříte první server a databázi. Hodnoty, které jste zadali, jsou ověřeny a nasazení spuštění.

    ![Nové databáze sql 17](./media/sql-database-get-started/sql-database-new-database-17.png)

20. Na portálu nástrojů klikněte na položky **upozornění** , které chcete zkontrolovat stav nasazení.

    ![Nové databáze sql 18](./media/sql-database-get-started/sql-database-new-database-18.png)

>[AZURE.IMPORTANT]Po dokončení nasazení nový Azure SQL server a databáze vytvořené v Azure. Nebudete moct připojit k serveru nové nebo databáze pomocí nástroje SQL Server, dokud nevytvoříte pravidlo pro otevření databáze SQL brány firewall pro připojení z mimo Azure firewall serveru.

[AZURE.INCLUDE [Create server firewall rule](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="next-steps"></a>Další kroky
Teď, když dokončíte tento kurz databáze SQL a vytvořili databáze s ukázkovými daty, jste připraveni na prozkoumání pomocí své oblíbené nástroje.

- Pokud znáte jazyce Transact-SQL a SQL Server Management Studio (SSMS), přečtěte si, jak [připojit a dotaz SQL databáze pomocí SSMS](sql-database-connect-query-ssms.md).

- Pokud víte, Excel, přečtěte si, jak se [připojit k databázi SQL Azure s Excelem](sql-database-connect-excel.md).

- Pokud jste připraveni začít kódování, zvolte programovací jazyk u [knihovny připojení databáze SQL a SQL Server](sql-database-libraries.md).

- Pokud chcete přesunout vaší místní databáze systému SQL Server Azure, najdete v článku [Migrace databáze k SQL databázi](sql-database-cloud-migrate.md) na další.

- Pokud chcete načíst některá data do nové tabulky ze souboru CSV pomocí nástroje příkazového řádku BCP, přečtěte si článek [načítání dat do databáze SQL pomocí souboru CSV pomocí BCP](sql-database-load-from-csv-with-bcp.md).

- Pokud chcete Seznamte se s ním zabezpečení databáze SQL Azure, přečtěte si článek [Začínáme s zabezpečení](sql-database-get-started-security.md)


## <a name="additional-resources"></a>Další zdroje informací

[Co je databáze SQL?](sql-database-technical-overview.md)
