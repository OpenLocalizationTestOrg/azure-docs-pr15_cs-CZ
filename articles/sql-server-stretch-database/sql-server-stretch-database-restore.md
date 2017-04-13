<properties
    pageTitle="Obnovování databází roztáhnout s podporou | Microsoft Azure"
    description="Zjistěte, jak obnovit roztáhnout\-povolené databází."
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
    ms.date="08/01/2016"
    ms.author="douglasl"/>

# <a name="restore-stretch-enabled-databases"></a>Obnovování databází roztáhnout s podporou

Obnovení zálohovaného databáze v případě potřeby obnovit z mnoha typů k chybám, chyb a havárie.

Další informace o zálohování najdete v článku [zálohy roztáhnout s podporou databází](sql-server-stretch-database-backup.md).

>   [AZURE.NOTE] Zálohování bylo jedinou část dokončení dostupnost a obchodního kontinuitu řešení. Další informace o dostupnost najdete v článku [Vysoké dostupnost řešení](https://msdn.microsoft.com/library/ms190202.aspx).

## <a name="restore-your-sql-server-data"></a>Obnovení data SQL serveru
Jak obnovit před selháním hardwaru nebo poškození obnovit roztáhnout\-povolené databáze SQL serveru ze zálohy. Můžete dál používat metody obnovení SQL serveru, které používáte. Další informace najdete v tématu [obnovení přehled a obnovení](https://msdn.microsoft.com/library/ms191253.aspx).

Po obnovení databáze SQL serveru, je třeba spustit uložená procedura **sys.sp_rda_reauthorize_db** znovu navázat spojení mezi roztáhnout\-povolené databáze SQL serveru a Vzdálená databáze Azure. Další informace najdete v tématu [obnovení připojení mezi databáze SQL serveru a Vzdálená databáze Azure](#restore-the-connection-between-the-sql-server-database-and-the-remote-azure-database).

## <a name="restore-your-remote-azure-data"></a>Obnovení vzdálené Azure dat

### <a name="recover-a-live-azure-database"></a>Obnovit databázi živou Azure
Databáze SQL serveru roztáhnout služby na Azure snímky všechny dynamických dat minimálně každých 8 hodin použití Azure úložiště snímků. Tyto snímky zachovány dobu 7 dní. Umožňuje provádět obnovíte na jeden z aspoň 21 bodů v čase v posledních 7 dnech až do doby, kdy byla přijatá poslední snímek.

Obnovení živou Azure databáze předchozí pomocí portálu Azure, proveďte následující akce.

1. Přihlaste se k portálu Azure.
2. Na levé straně obrazovky vyberte **Procházet** a vyberte **SQL databáze**.
3. Přejděte do vaší databáze a vyberte ho.
4. V horní části zásuvné databázi klikněte na **Obnovit**.
5. Zadejte nový **název databáze**, vyberte **Obnovit čárky** a klikněte na **vytvořit**.
6. Proces obnovení databáze začne a můžete sledovat pomocí **oznámení**.

### <a name="recover-a-deleted-azure-database"></a>Obnovení odstraněné Azure databáze
Služba roztáhnout databáze systému SQL Server na Azure přebírat snímek databáze před databáze je nezobrazí a zachová dobu 7 dní. Když k tomu dojde, už zachová snímky z živého databáze. Toto oprávnění umožňuje obnovit odstraněnou databázi k bodu po odstranění.

Obnovení odstraněné Azure databáze po odstranění pomocí portálu Azure, proveďte následující kroky.

1. Přihlaste se k portálu Azure.
2. Na levé straně obrazovky vyberte **Procházet** a vyberte **Servery SQL**.
3. Přejděte na serveru a vyberte ho.
4. Posuňte se dolů na váš server zásuvné samostatných, klikněte na dlaždici **Odstranit databáze** .
5. Vyberte odstraněné databázi, kterou chcete obnovit.
5. Zadejte nový **název databáze** a klikněte na **vytvořit**.
6. Proces obnovení databáze začne a můžete sledovat pomocí **oznámení**.

## <a name="restore-the-connection-between-the-sql-server-database-and-the-remote-azure-database"></a>Obnovení připojení mezi databáze SQL serveru a Vzdálená databáze Azure

1.  Pokud budete chtít připojení k databázi obnovená Azure s jiným názvem nebo v jiné oblasti, spusťte uložená procedura [sys.sp_rda_deauthorize_db](https://msdn.microsoft.com/library/mt703716.aspx) odpojit od předchozí Azure databázi.  

2.  Spuštění uložené procedury [sys.sp_rda_reauthorize_db](https://msdn.microsoft.com/library/mt131016.aspx) připojit místní roztáhnout\-povolené databáze do Azure databáze.  

    -   Poskytnout existující pověření databáze s rozsahem jako sysname nebo datová\(128\) hodnotu. \(Nepoužívejte datová\(maximální\).\) Můžete vyhledat názvu přihlašovacích údajů v zobrazení **sys.database\_omezené\_pověření**.  

    -   Určete, zda chcete vytvořit kopii Vzdálená data a připojte kopii (doporučeno).  

    ```tsql  
    USE <Stretch-enabled database name>;
    GO
    EXEC sp_rda_reauthorize_db
        @credential = N'<existing_database_scoped_credential_name>',
        @with_copy = 1 ;  
    GO
    ```  

## <a name="see-also"></a>Viz taky

[Správa a řešení potíží s roztáhnout databáze](sql-server-stretch-database-manage.md)

[Sys.sp_rda_reauthorize_db (Transact-SQL)](https://msdn.microsoft.com/library/mt131016.aspx)

[Zálohování a obnovení databáze SQL serveru](https://msdn.microsoft.com/library/ms187048.aspx)
