<properties
    pageTitle="Archivace databáze Azure SQL do souboru BACPAC pomocí portálu Azure"
    description="Archivace databáze Azure SQL do souboru BACPAC pomocí portálu Azure"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/15/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="archive-an-azure-sql-database-to-a-bacpac-file-using-the-azure-portal"></a>Archivace databáze Azure SQL do souboru BACPAC pomocí portálu Azure

> [AZURE.SELECTOR]
- [Azure portálu](sql-database-export.md)
- [Prostředí PowerShell](sql-database-export-powershell.md)

Tento článek obsahuje pokyny pro archivaci databáze Azure SQL do souboru BACPAC (uložené v úložišti objektů blob Azure) pomocí [Azure portálu](https://portal.azure.com).

Když je potřeba vytvořit archivu databáze Azure SQL, můžete exportovat do souboru BACPAC schéma databáze a data. Soubor BACPAC je jednoduše ZIP soubor s příponou BACPAC. Soubor BACPAC můžete později uložené v úložišti objektů blob Azure nebo místní úložiště na místního pracoviště a později importovaných zpět do databáze SQL Azure nebo do SQL Server místní instalace. 

***Co byste měli zvážit***

- Archivace využití transakce konzistentní, je nutné zajistit buď bez zápisu aktivity dochází při exportu nebo který exportujete ze [využití transakce konzistentní kopii](sql-database-copy.md) databáze Azure SQL.
- Maximální velikost souboru BACPAC archivace k úložišti objektů Blob Azure je 200 GB. Archivace větší soubor BACPAC k místním úložišti, nástroj [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) příkazový řádek. Tento nástroj se dodává s Visual Studia a SQL Server. Také je možné [Stáhnout](https://msdn.microsoft.com/library/mt204009.aspx) nejnovější verzi SQL Server Data Tools získat tento nástroj.
- Archivace k základnímu úložišti Azure premium pomocí souboru BACPAC není podporovaná.
- Pokud operace exportu překročí 20 hodin, může ji zrušit. Pokud chcete zvýšit výkon při exportu, máte tyto možnosti:
 - Dočasně zvýšit úroveň služby.
 - Ukončení všechny čtení a zápis aktivity při exportu.
 - Použití [seskupený index](https://msdn.microsoft.com/library/ms190457.aspx) s jinou hodnotu než null na všechny rozsáhlých tabulek. Bez skupinový indexy export nemusí podařit, pokud trvá déle než 6-12 hodin. Je to proto službu exportovat je potřeba dokončit prohledávání tabulky při exportu celou tabulku. Spolehlivých způsobů, jak zjistit, pokud jsou tabulkách optimalizované pro export je spustit **DBCC SHOW_STATISTICS** a ověřte, že *RANGE_HI_KEY* není null jeho hodnotu má dobré rozdělení. Podrobnosti najdete v tématu [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).


> [AZURE.NOTE] BACPACs nejsou určeny se dá použít pro zálohování a obnovení. Databáze SQL Azure automaticky vytvoří zálohy pro každý uživatel databáze. Další informace najdete v tématu [Přehled kontinuitu Business](sql-database-business-continuity.md).

Tento článek je potřeba k provedení těchto věcí:

- Předplatné Azure.
- Databáze Azure SQL. 
- Pro [účet Azure standardní úložiště](../storage/storage-create-storage-account.md) objektů blob kontejneru ukládat BACPAC v standardní úložiště.

## <a name="export-your-database"></a>Export databáze

Otevřete zásuvné databáze SQL databáze, kterou chcete exportovat.

> [AZURE.IMPORTANT] Chcete-li zajistit souboru využití transakce konzistentní BACPAC doporučujeme první [vytvořit kopii databáze](sql-database-copy.md) a potom soubor vyexportujte kopie databáze. 

1.  Přejděte na [portál Azure](https://portal.azure.com).
2.  Klikněte na **SQL databáze**.
3.  Klikněte na databáze, kterou chcete archivovat.
4.  V SQL databáze zásuvné klikněte na **Exportovat** zobrazíte zásuvné **Export databáze** :

    ![tlačítko Exportovat][1]

5.  Klikněte na **úložiště** a vyberte svůj účet a objektů blob kontejner úložiště uložení BACPAC:

    ![Export databáze][2]

6. Vyberte typ ověřování. 
7.  Zadejte příslušný přihlašovací údaje pro server Azure SQL obsahující databáze, který exportujete.
8.  Klikněte na tlačítko **OK** k archivaci databáze. Kliknutím na tlačítko **OK** vytvoří požadavek databáze exportovat a odevzdá ho ke službě. Časový interval, který bude trvat exportovat závisí na velikosti a složitosti databáze a úroveň služby. Zobrazí se oznámení.

    ![Export upozornění][3]

## <a name="monitor-the-progress-of-the-export-operation"></a>Sledování průběhu operace exportu

1.  Klikněte na položku **SQL servery**.
2.  Klikněte na název serveru, jehož původní databázi (zdrojový), kterou právě archivovat.
3.  Posuňte se dolů na operace.
4.  V SQL server zásuvné klikněte na **Import nebo Export historie**:

    ![Import export historie][4]

## <a name="verify-the-bacpac-is-in-your-storage-container"></a>Ověření, že je BACPAC v kontejneru úložiště

1.  Klikněte na **úložiště účty**.
2.  Klikněte na úložiště účet, který jste uložili na možnost Archivovat BACPAC.
3.  Klikněte na **kontejnery** a vyberte kontejneru jste exportovali databáze do podrobnosti (který budou moct stáhnout a uložit BACPAC postupovat dál?).

    ![Podrobnosti o .bacpac souboru][5]  

## <a name="next-steps"></a>Další kroky

- Další informace o importu BACPAC k databázi SQL Azure, přečtěte si téma [Import BACPCAC k databázi Azure SQL](sql-database-import.md)
- Další informace o importu BACPAC k databázi SQL serveru, přečtěte si téma [Import BACPCAC k databázi SQL serveru](https://msdn.microsoft.com/library/hh710052.aspx)



<!--Image references-->
[1]: ./media/sql-database-export/export.png
[2]: ./media/sql-database-export/export-blade.png
[3]: ./media/sql-database-export/export-notification.png
[4]: ./media/sql-database-export/export-history.png
[5]: ./media/sql-database-export/bacpac-archive.png

