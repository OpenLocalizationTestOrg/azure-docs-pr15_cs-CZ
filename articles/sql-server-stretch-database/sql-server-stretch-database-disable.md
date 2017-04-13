<properties
    pageTitle="Zakázání roztáhnout databáze a pak znovu Vzdálená data | Microsoft Azure"
    description="Zjistěte, jak zakázat roztáhnout databáze pro tabulky a volitelně pak znovu Vzdálená data."
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
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="disable-stretch-database-and-bring-back-remote-data"></a>Zakázání roztáhnout databáze a pak znovu Vzdálená data

Jak zakázat roztáhnout databáze pro tabulku, vyberte **Roztáhnout** pro tabulky v SQL Server Management Studio. Vyberte jednu z následujících možností.

-   Jak zakázat **| Vrátit dat z Azure**. Kopírovat Vzdálená data pro tabulku z Azure zpět k SQL serveru, pak zakázat roztáhnout databáze pro tabulku. Tuto operaci poněkud náklady přenos dat a nelze ji zrušit.

-   Jak zakázat **| Nechte dat v Azure**. Zakázání roztáhnout databáze pro tabulku.  Opustit Vzdálená data pro tabulky v Azure.

Můžete taky použít Transact\-SQL zakázání roztáhnout databázi, tabulku nebo pro danou databázi.

Po zakážete roztáhnout databáze pro tabulku, přestane migraci dat a výsledků dotazu už obsahovat výsledky vzdálené tabulky.

Pokud chcete jednoduše podržte myš migraci dat, přečtěte si článek [Pozastavení a obnovení roztáhnout databáze](sql-server-stretch-database-pause.md).

>   [AZURE.NOTE] Zakázání roztáhnout databáze pro tabulku nebo pro danou databázi neodstraní vzdálený objekt. Pokud chcete odstranit vzdálené tabulky nebo vzdálené databázi, budete muset pusťte pomocí portálu pro správu Azure. Vzdálené objekty dál vynakládá Azure, dokud je odstranit. Další informace najdete v tématu [SQL Server roztáhnout databáze cena za](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

## <a name="disable-stretch-database-for-a-table"></a>Zakázání roztáhnout databáze pro tabulku

### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-table"></a>Zakázání roztáhnout databáze pro tabulku pomocí SQL Server Management Studio

1.  V SQL Server Management Studio v Průzkumníku objekt vyberte tabulku, u kterého chcete zakázat roztáhnout databáze.

2.  Vpravo\-klikněte na a vyberte **Roztáhnout**a pak vyberte jednu z následujících možností.

    -   Jak zakázat **| Vrátit dat z Azure**. Kopírovat Vzdálená data pro tabulku z Azure zpět k SQL serveru, pak zakázat roztáhnout databáze pro tabulku. Tento příkaz se nedá zrušit.

        >   [AZURE.NOTE] Kopírování Vzdálená data tabulky z Azure poněkud zpátky na serveru SQL Server data přenos náklady. Další informace najdete v tématu [Podrobnosti ceny přenosy dat](https://azure.microsoft.com/pricing/details/data-transfers/).

        Po všechny zkopíroval Vzdálená data z Azure zpět na serveru SQL Server, roztáhnout není k dispozici pro tabulku.

    -   Jak zakázat **| Nechte dat v Azure**. Zakázání roztáhnout databáze pro tabulku.  Opustit Vzdálená data pro tabulky v Azure.

    >   [AZURE.NOTE] Zákaz roztáhnout databáze pro tabulky se neodstraní Vzdálená data nebo vzdálené tabulky. Pokud chcete odstranit vzdálené tabulky, budete muset pusťte pomocí portálu pro správu Azure. Vzdálené tabulky i nadále vynakládá Azure dokud neodstraníte ho. Další informace najdete v tématu [SQL Server roztáhnout databáze cena za](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

### <a name="use-transact-sql-to-disable-stretch-database-for-a-table"></a>Použití Transact\-SQL zakázat roztáhnout databáze pro tabulky

-   Zakažte roztáhnout pro tabulky a zkopírujte vzdáleného zpět dat tabulky z Azure SQL Server, spusťte tento příkaz. Po všechny zkopíroval Vzdálená data z Azure zpět na serveru SQL Server, roztáhnout není k dispozici pro tabulku.

    Tento příkaz se nedá zrušit.

    ```tsql
    USE <Stretch-enabled database name>;
    GO
    ALTER TABLE <Stretch-enabled table name>  
       SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;
    GO
    ```
    >   [AZURE.NOTE] Kopírování Vzdálená data tabulky z Azure poněkud zpátky na serveru SQL Server data přenos náklady. Další informace najdete v tématu [Podrobnosti ceny přenosy dat](https://azure.microsoft.com/pricing/details/data-transfers/).

-   Zakázání roztáhnout pro tabulky a opustit Vzdálená data, spusťte tento příkaz.

    ```tsql
    ALTER TABLE <table_name>
       SET ( REMOTE_DATA_ARCHIVE = OFF_WITHOUT_DATA_RECOVERY ( MIGRATION_STATE = PAUSED ) ) ;
    ```

>   [AZURE.NOTE] Zákaz roztáhnout databáze pro tabulky se neodstraní Vzdálená data nebo vzdálené tabulky. Pokud chcete odstranit vzdálené tabulky, budete muset pusťte pomocí portálu pro správu Azure. Vzdálené tabulky i nadále vynakládá Azure dokud neodstraníte ho. Další informace najdete v tématu [SQL Server roztáhnout databáze cena za](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

## <a name="disable-stretch-database-for-a-database"></a>Zakázání roztáhnout databáze pro danou databázi
Než roztáhnout databáze můžete zakázat pro danou databázi, musíte zakázat roztáhnout databáze na jednotlivé roztáhnout\-povolené tabulek v databázi.

### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-database"></a>Zakázání roztáhnout databáze pro danou databázi pomocí SQL Server Management Studio

1.  V SQL Server Management Studio v Průzkumníku objekt vyberte databázi, pro kterou chcete zakázat roztáhnout databáze.

2.  Vpravo\-klikněte na a vyberte **úkoly**, vyberte **Roztáhnout**a potom zaškrtněte políčko **Zakázat**.

>   [AZURE.NOTE] Zakázání roztáhnout databáze pro danou databázi neodstraní vzdálené databázi. Pokud chcete odstranit Vzdálená databáze, budete muset pusťte pomocí portálu pro správu Azure. Vzdálená databáze zůstane vynakládá Azure dokud neodstraníte ho. Další informace najdete v tématu [SQL Server roztáhnout databáze cena za](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

### <a name="use-transact-sql-to-disable-stretch-database-for-a-database"></a>Použití Transact\-SQL zakázat roztáhnout databáze pro danou databázi
Spusťte tento příkaz.

```tsql
ALTER DATABASE <database name>
    SET REMOTE_DATA_ARCHIVE = OFF ;
```

>   [AZURE.NOTE] Zakázání roztáhnout databáze pro danou databázi neodstraní vzdálené databázi. Pokud chcete odstranit Vzdálená databáze, budete muset pusťte pomocí portálu pro správu Azure. Vzdálená databáze zůstane vynakládá Azure dokud neodstraníte ho. Další informace najdete v tématu [SQL Server roztáhnout databáze cena za](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

## <a name="see-also"></a>Viz taky

[Příkaz ALTER databáze nastavení možností (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx)

[Pozastavit roztáhnout databáze](sql-server-stretch-database-pause.md)
