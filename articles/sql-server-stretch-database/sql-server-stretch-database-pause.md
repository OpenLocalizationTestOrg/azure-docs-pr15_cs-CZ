<properties
    pageTitle="Pozastavit migraci dat (roztáhnout databáze) | Microsoft Azure"
    description="Naučte se pozastavit nebo obnovit migrace dat z Azure."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/14/2016"
    ms.author="douglasl"/>

# <a name="pause-and-resume-data-migration-stretch-database"></a>Pozastavit migraci dat (roztáhnout databáze)

Pozastavení nebo obnovení migrace dat z Azure, vyberte **Roztáhnout** pro tabulky v SQL Server Management Studio a klikněte na **Pozastavit** pozastavit migraci dat nebo **obnovení** pokračovat migraci dat. Můžete taky použít Transact\-SQL pozastavení nebo obnovení dat migrace.

Pozastavit migraci dat u jednotlivých tabulek, pokud chcete řešení problémů s místním serverem nebo maximalizovat dostupné šířka pásma.

## <a name="pause-data-migration"></a>Pozastavit migraci dat

### <a name="use-sql-server-management-studio-to-pause-data-migration"></a>Zastavte ukazatel myši migraci dat pomocí SQL Server Management Studio

1.  V SQL Server Management Studio v Průzkumníku objekt vyberte roztáhnout\-povolené tabulku, u kterého chcete pozastavit migraci dat.

2.  Vpravo\-klikněte na a vyberte **Roztáhnout**a klikněte na **Pozastavit**.

### <a name="use-transact-sql-to-pause-data-migration"></a>Použití Transact\-SQL pozastavit migraci dat
Spusťte tento příkaz.

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>  
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = PAUSED ) ) ;  
GO
```

## <a name="resume-data-migration"></a>Obnovení dat migrace

### <a name="use-sql-server-management-studio-to-resume-data-migration"></a>Použití SQL Server Management Studio pokračovat migraci dat

1.  V SQL Server Management Studio v Průzkumníku objekt vyberte roztáhnout\-povolené tabulky, pro kterou chcete obnovit migraci dat.

2.  Vpravo\-klikněte na a vyberte **Roztáhnout**a pak vyberte **Obnovit**.

### <a name="use-transact-sql-to-resume-data-migration"></a>Použití Transact\-SQL pokračovat migraci dat
Spusťte tento příkaz.

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>   
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = OUTBOUND ) ) ;  
 GO
```

## <a name="check-whether-migration-is-active-or-paused"></a>Kontrola, jestli migrace aktivní nebo pozastaveném ukládání

### <a name="use-sql-server-management-studio-to-check-whether-migration-is-active-or-paused"></a>Umožňuje zkontrolovat, zda je migrace aktivní nebo pozastaveném SQL Server Management Studio
V SQL Server Management Studio otevřete **Sledování roztáhnout databáze** a (doména) hodnotu ve sloupci **Stav migrace** . Další informace najdete v tématu [sledování a odstraňování problémů s migrací dat](sql-server-stretch-database-monitor.md).

### <a name="use-transact-sql-to-check-whether-migration-is-active-or-paused"></a>Umožňuje zkontrolovat, zda je migrace aktivní nebo pozastaveném jazyce Transact-SQL
Dotaz zobrazení katalogu **sys.remote_data_archive_tables** a (doména) hodnotu ve sloupci **is_migration_paused** . Další informace najdete v tématu [sys.remote_data_archive_tables](https://msdn.microsoft.com/library/dn935003.aspx).

## <a name="see-also"></a>Viz taky

[ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx)
[sledování a odstraňování problémů s migrací dat](sql-server-stretch-database-monitor.md)
