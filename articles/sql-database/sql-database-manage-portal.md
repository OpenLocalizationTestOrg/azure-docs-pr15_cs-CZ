<properties
    pageTitle="Správa databáze SQL Azure pomocí portálu Azure | Microsoft Azure"
    description="Naučte se používat portál Azure ke správě relační databáze v cloudu pomocí portálu Azure."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.date="09/19/2016"
    ms.author="sstein"/>


# <a name="managing-azure-sql-databases-using-the-azure-portal"></a>Správa databáze SQL Azure pomocí portálu Azure


> [AZURE.SELECTOR]
- [Azure portálu](sql-database-manage-portal.md)
- [SSMS](sql-database-manage-azure-ssms.md)
- [Prostředí PowerShell](sql-database-manage-powershell.md)

[Azure portál](https://portal.azure.com/) umožňuje vytvářet, sledovat a spravovat databáze Azure SQL a servery. Tento článek obsahuje odkazy na podrobné informace o běžné úlohy a stručný popis.

## <a name="view-your-azure-sql-databases-servers-and-pools"></a>Zobrazení databáze Azure SQL, servery a skupin

Pokud chcete zobrazit službám databáze SQL, klikněte na **Další služby**a do vyhledávacího pole zadejte **SQL** :

![Databáze SQL](./media/sql-database-manage-portal/sql-services.png)


## <a name="how-do-i-create-or-view-azure-sql-databases"></a>Jak vytvořit nebo zobrazit databáze Azure SQL?

Otevřete zásuvné **SQL databáze** , klikněte na **SQL databáze**a klikněte na databázi, kterou chcete pracovat s nebo klikněte na tlačítko **+ Přidat** k vytvoření databáze SQL. Další informace najdete v tématu [vytvoření databáze SQL v minutách pomocí portálu Azure](sql-database-get-started.md).


![Databáze SQL](./media/sql-database-manage-portal/sql-databases.png)


## <a name="how-do-i-create-or-view-azure-sql-servers"></a>Jak vytvořit nebo zobrazit servery Azure SQL?

Otevřete zásuvné **servery SQL** , klikněte na položku **servery SQL**a klikněte na serveru, ke kterému chcete pracovat s nebo klikněte na tlačítko **+ Přidat** vytvořit SQL server. Další informace najdete v tématu [vytvoření databáze SQL v minutách pomocí portálu Azure](sql-database-get-started.md).

![Servery SQL](./media/sql-database-manage-portal/sql-servers.png)


## <a name="how-do-i-create-or-view-sql-elastic-pools"></a>Jak vytvořit nebo zobrazit pružná fondů SQL?

Otevřete zásuvné **pružná fondů SQL** , klikněte **pružná fondů SQL**a klikněte na skupiny, která chcete pracovat s nebo klikněte na tlačítko **+ Přidat** vytvoření fondu. Další informace najdete v tématu [Vytvoření fondu pružná databáze pomocí portálu Azure](sql-database-elastic-pool-create-portal.md).

![Pružná fondů SQL](./media/sql-database-manage-portal/elastic-pools.png)



## <a name="how-do-i-update-or-view-sql-database-settings"></a>Jak aktualizovat nebo zobrazit nastavení databáze SQL?

Chcete-li zobrazit nebo aktualizovat nastavení databáze, klikněte na požadované nastavení na zásuvné databáze SQL:


![Nastavení databáze SQL](./media/sql-database-manage-portal/settings.png)


## <a name="how-do-i-find-a-sql-databases-fully-qualified-server-name"></a>Jak najdu databáze plně kvalifikovaný název serveru SQL Server?

Pokud chcete zobrazit název svého serveru databáze, klikněte na **Přehled** na zásuvné **databáze SQL** a zapište si název serveru:


![Nastavení databáze SQL](./media/sql-database-manage-portal/server-name.png)


## <a name="how-do-i-manage-firewall-rules-to-control-access-to-my-sql-server-and-database"></a>Jak můžu spravovat pravidla brány firewall pro řízení přístupu k Moje serveru SQL server a databázi?

Zobrazení, vytvoření nebo aktualizace pravidel brány firewall, klikněte na **nastavení brány firewall serveru** na zásuvné **databáze SQL** . Další informace najdete v tématu [Konfigurace brány firewall na úrovni serveru pravidlo databáze SQL Azure pomocí portálu Azure](sql-database-configure-firewall-settings.md).


![pravidla brány firewall](./media/sql-database-manage-portal/sql-database-firewall.png)


## <a name="how-do-i-change-my-sql-database-service-tier-or-performance-level"></a>Jak změnit svůj SQL databáze služby osy úroveň výkonu a?


Služby osy nebo výkonu úroveň databázi SQL, klepnutím na tlačítko **vrstvy ceny (měřítko DTUs)** na zásuvné **databáze SQL** . Podrobnosti najdete v tématu [Změna služby osy a výkonu úrovně (cena za osy) databáze SQL](sql-database-scale-up.md).


![ceny úrovní](./media/sql-database-manage-portal/pricing-tier.png)


## <a name="how-do-i-configure-auditing-and-threat-detection-for-a-sql-database"></a>Jak nakonfigurovat auditování a hrozeb zjišťování databáze SQL?

Konfigurace auditování a hrozeb zjišťování databáze SQL, klikněte na **zjišťování auditování a hrozbou, že** na zásuvné **databáze SQL** . Podrobnosti najdete v tématu [Začínáme s auditování databáze SQL](sql-database-auditing-get-started.md)a [začít pracovat s zjišťování hrozbou, že databáze SQL](sql-database-threat-detection-get-started.md).


## <a name="how-do-i-configure-dynamic-data-masking-for-a-sql-database"></a>Jak nakonfigurovat dynamických dat maskování databáze SQL?

Abyste mohli nakonfigurovat dynamických dat maskování databáze SQL, klikněte na zásuvné **databáze SQL** **dynamických dat maskování** . Podrobnosti najdete v tématu [Začínáme s SQL databáze dynamických dat maskování](sql-database-dynamic-data-masking-get-started.md).


## <a name="how-do-i-configure-transparent-data-encryption-tde-for-a-sql-database"></a>Jak nakonfigurovat průhledné šifrování (TDE) pro danou databázi SQL?

Abyste mohli nakonfigurovat průhledné šifrování databáze SQL, klikněte na zásuvné **databáze SQL** **průhledné šifrování** . Podrobnosti najdete v tématu [Povolení TDE na databázi na portálu](https://msdn.microsoft.com/library/dn948096#Anchor_1).

## <a name="how-do-i-view-or-change-the-max-size-of-a-sql-database"></a>Jak můžu zobrazit nebo změnit maximální velikost databáze SQL?

Chcete-li zobrazit nebo změnit velikost databáze SQL, klikněte na **velikost databáze** na zásuvné **databáze SQL** . Aktualizujte maximální velikost databáze změnou úroveň osy nebo výkonu služby. Podrobnosti najdete v tématu [Změna služby osy a výkonu úrovně (cena za osy) databáze SQL](sql-database-scale-up.md).

## <a name="how-do-i-monitor-and-improve-the-performance-of-a-sql-database"></a>Jak sledovat a vylepšení výkonu databázi SQL?

Sledování a vylepšení výkonu vlastnosti databáze SQL, klikněte na **výkon přehled** na zásuvné **databáze SQL** . Podrobnosti najdete v tématu [Přehled výkonu databáze SQL](sql-database-performance.md).


## <a name="how-do-i-configure-geo-replication"></a>Jak nakonfigurovat Geo replikace?

Pokud chcete nastavit Geo replikace databáze SQL, klikněte na zásuvné **databáze SQL** **Geo replikace** . Podrobnosti najdete v tématu [Konfigurace Geo replikace databáze SQL Azure pomocí portálu Azure](sql-database-geo-replication-portal.md).


## <a name="how-do-i-failover-to-a-geo-replicated-sql-database"></a>Postup převzetí k SQL databázi replikovat geo?

K přepnutí do vedlejší replikovat geo **Geo replikace** **databáze SQL** zásuvné, klepnutím na **převzetí**. Podrobnosti najdete v tématu [Zahájit plánované nebo neplánované selhání databáze SQL Azure pomocí portálu Azure](sql-database-geo-replication-failover-portal.md).


## <a name="how-do-i-copy-a-sql-database"></a>Jak zkopírovat databázi SQL?

**Pokud chcete zkopírovat databázi SQL, klepněte na zásuvné **databáze SQL** .** Další informace najdete v tématu [kopírování databáze Azure SQL pomocí portálu Azure](sql-database-copy-portal.md).


![Nastavení databáze SQL](./media/sql-database-manage-portal/sql-database-copy.png)

## <a name="how-do-i-archive-an-azure-sql-database-to-a-bacpac-file"></a>Jak archivace databáze Azure SQL do souboru BACPAC?

Pokud chcete vytvořit BACPAC databázi SQL, klepněte na zásuvné **databáze SQL** **Exportovat** . Podrobnosti najdete v tématu [archivu databáze Azure SQL do souboru BACPAC pomocí portálu Azure](sql-database-export.md).


![Export databáze SQL](./media/sql-database-manage-portal/sql-database-export.png)



## <a name="how-do-i-restore-a-sql-database-to-a-previous-point-in-time"></a>Jak obnovím databázi SQL na předchozí bod v čase?

Pokud chcete obnovit databázi SQL, klepněte na tlačítko **Obnovit** na zásuvné **databáze SQL** . Další informace najdete v tématu [obnovení databáze SQL Azure k předchozí bodu v čase pomocí portálu Azure](sql-database-point-in-time-restore-portal.md).


![Nastavení databáze SQL](./media/sql-database-manage-portal/sql-database-restore.png)


## <a name="how-do-i-create-an-azure-sql-database-from-a-bacpac-file"></a>Jak vytvořit databázi Azure SQL ze souboru BACPAC?

Pokud chcete vytvořit databázi SQL BACPAC souboru, klikněte na **Import z databáze** na zásuvné **SQL serveru** . Podrobnosti najdete v tématu [Import souboru BACPAC vytvoření databáze Azure SQL](sql-database-import.md).


![SQL server](./media/sql-database-manage-portal/server-commands.png)


## <a name="how-do-i-restore-a-deleted-sql-database"></a>Jak obnovit odstraněné databáze SQL?

Obnovení odstraněné SQL databáze, klikněte na **Odstranit databáze** na zásuvné **serveru SQL server** (SQL server, který obsahoval databáze, který byl odstraněn). Podrobnosti najdete v tématu [Obnovení odstraněných databáze Azure SQL pomocí portálu Azure](sql-database-restore-deleted-database-portal.md).

## <a name="how-do-i-delete-a-sql-database"></a>Jak odstranit databázi SQL?

Pokud chcete odstranit databázi SQL, klepněte na tlačítko **Odstranit** na zásuvné **databáze SQL** . 

![Nastavení databáze SQL](./media/sql-database-manage-portal/sql-database-delete.png)



## <a name="additional-resources"></a>Další zdroje informací

- [Databáze SQL](sql-database-technical-overview.md)
- [Sledování a správa fondu pružná databáze pomocí portálu Azure](sql-database-elastic-pool-manage-portal.md)
