<properties
    pageTitle="Povolení roztáhnout databáze pro danou databázi | Microsoft Azure"
    description="Informace o konfiguraci databáze pro roztáhnout databázi."
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

# <a name="enable-stretch-database-for-a-database"></a>Povolení roztáhnout databáze pro danou databázi

Abyste mohli nakonfigurovat existující databáze pro databázi roztáhnout, vyberte úkoly **| Roztáhnout | Povolení** pro databáze systému SQL Server Management Studio otevřete průvodce **Povolit databáze pro roztáhnout** . Můžete taky použít Transact\-SQL povolit roztáhnout databáze pro danou databázi.

Pokud vyberete **úkoly | Roztáhnout | Povolení** pro jednotlivé tabulky a ještě nepovolili databáze pro databázi roztáhnout, Průvodce nakonfiguruje databázi pro roztáhnout databázi a umožňuje výběr tabulky jako součást procesu. Postupujte podle kroků v tomto tématu místo kroků v tématu [Povolení roztáhnout databáze pro tabulku](sql-server-stretch-database-enable-database.md).

Povolení roztáhnout databáze na databázi nebo tabulky vyžaduje db\_oprávnění vlastníka. Povolení roztáhnout databáze na databázi taky vyžaduje oprávnění k databázi ovládací PRVEK.

 >   [AZURE.NOTE] Později Pokud zakážete roztáhnout databáze, mějte na paměti, že zakážete roztáhnout databáze pro tabulky nebo databáze se neodstraní vzdálený objekt. Pokud chcete odstranit vzdálené tabulky nebo vzdálené databázi, budete muset pusťte pomocí portálu pro správu Azure. Vzdálené objekty dál vynakládá Azure, dokud neodstraníte ručně.

## <a name="before-you-get-started"></a>Než začnete

-   Před zahájením konfigurace databáze pro roztáhnout, doporučujeme spusťte Poradce roztáhnout databáze k identifikaci databází a tabulek, které se dá použít roztáhnout. Konference Advisor databáze roztáhnout také identifikuje blokování problémy. Další informace najdete v tématu [Představení databází a tabulek pro databázi roztáhnout](sql-server-stretch-database-identify-databases.md).

-   Prohlédněte si [omezení pro roztáhněte databáze](sql-server-stretch-database-limitations.md).

-   Roztažení databáze migruje dat Azure. Proto se musí mít účet Azure a předplatné k fakturaci. Chcete-li získat Azure účet, [klikněte sem](http://azure.microsoft.com/pricing/free-trial/).

-   Jste připojení a přihlašovací informace, které potřebujete k vytvoření nového Azure serveru nebo vyberte stávající server Azure.

## <a name="EnableTSQLServer"></a>Předpoklady: Povolení roztáhnout databáze na serveru
Dříve než povolíte roztáhnout databáze na databázi nebo tabulky budete muset povolit místního serveru. Tato operace vyžaduje oprávnění členem nebo serveradmin.

-   Pokud máte potřebná oprávnění pro správu, průvodce **Povolit databáze pro roztáhnout** konfiguruje server pro roztáhnout.

-   Pokud nemáte potřebná oprávnění, Správce musí povolit možnost ručně spuštěním **sp\_konfigurace** před spuštěním průvodce nebo správce má k spusťte průvodce.

Pokud chcete povolit, roztáhnout databáze na serveru ručně, spusťte **sp\_konfigurace** a zapněte možnost **Archivovat Vzdálená data** . Následující příklad umožňuje možnost **Archivovat Vzdálená data** nastavením jeho hodnotu na 1.

```
EXEC sp_configure 'remote data archive' , '1';
GO

RECONFIGURE;
GO
```
Další informace najdete v tématu [Konfigurace Vzdálená data archivace možnost konfigurace serveru](https://msdn.microsoft.com/library/mt143175.aspx) a [sp_configure jazyce Transact-SQL ()](https://msdn.microsoft.com/library/ms188787.aspx).

## <a name="Wizard"></a>Použití průvodce k povolení roztáhnout databáze na databázi
Informace o povolení databázi Průvodce roztáhnout včetně informace, které musíte zadat možnosti, které je potřeba provést najdete v článku [Začínáme databáze povolit roztáhnout Průvodce spuštěním](sql-server-stretch-database-wizard.md).

## <a name="EnableTSQLDatabase"></a>Použití Transact\-SQL povolíte roztáhnout databáze u databáze
Dříve než povolíte roztáhnout databáze na jednotlivé tabulky budete muset povolit databázi.

Povolení roztáhnout databáze na databázi nebo tabulky vyžaduje db\_oprávnění vlastníka. Povolení roztáhnout databáze na databázi taky vyžaduje oprávnění k databázi ovládací PRVEK.

1.  Než začnete, vyberte existující server Azure pro data, která migruje roztáhnout databáze nebo vytvořte nový server Azure.

2.  Na Azure server vytvořte pravidlo brány firewall kombinací rozsah IP adresu serveru SQL Server, který umožňuje serveru SQL Server komunikovat s vzdálený server.

    Vyhledání hodnot je to potřeba a vytvoříte pravidlo brány firewall pokoušející se připojit k serveru daného Azure z Průzkumníka objekt v SQL Server Management Studio (SSMS). SSMS vám pomůže vytvořit pravidlo otevřením následující dialogovým oknem obsahující požadované hodnoty IP adres.

    ![Vytvoření pravidla brány firewall v SSMS][FirewallRule]

3.  Konfigurace databáze SQL serveru pro databázi roztáhnout, musí mít hlavní klíč databáze databáze. Hlavní klíč databáze zabezpečuje přihlašovací údaje, které roztáhnout databázi používá pro připojení ke vzdálené databázi. Tady je příklad vytvoří nový hlavní klíč databáze.

    ```tsql
    USE <database>;
    GO

    CREATE MASTER KEY ENCRYPTION BY PASSWORD ='<password>';
    GO
    ```

    Další informace o klíči hlavní databáze najdete v článku [vytvoření hlavní klíč (Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) a [vytvořit hlavní klíč databáze](https://msdn.microsoft.com/library/aa337551.aspx).

4.  Při konfiguraci databáze pro databázi roztáhnout, budete muset zadat pověření pro databázi roztáhnout používat ke komunikaci mezi na místní SQL serveru a vzdálený server Azure. Máte dvě možnosti.

    -   Můžete zadat přihlašovací údaje správce.

        -   Pokud povolíte roztáhnout databáze pomocí průvodce, můžete vytvořit pověření v té době.

        -   Pokud chcete povolit roztáhnout databáze spuštěním **Vlastnosti databáze**, musíte vytvořit pověření ručně před spuštěním **Vlastnosti databáze** povolit roztáhnout databáze.

        Tady je příklad vytvoří nové přihlašovací údaje.

        ```tsql
        CREATE DATABASE SCOPED CREDENTIAL <db_scoped_credential_name>
            WITH IDENTITY = '<identity>' , SECRET = '<secret>';
        GO
        ```

        Další informace o pověření najdete v článku [Vytvoření databáze omezené přihlašovacích údajů (Transact-SQL)](https://msdn.microsoft.com/library/mt270260.aspx). Vytvoření pověření vyžaduje oprávnění upravit jakékoli pověření.

    -   Účet federované služby pro systém SQL Server můžete komunikovat s vzdálený server Azure, pokud jsou splněny všechny následující podmínky.

        -   Účet služby, pod kterým je spuštěný instanci systému SQL Server je doménovým účtem.

        -   Doménovým účtem patří domény u služby Active Directory je federované se službou Azure Active Directory.

        -   Vzdálený server Azure konfigurace podporuje ověřování služby Azure Active Directory.

        -   Účet služby, pod kterým je spuštěn SQL serveru musí být nakonfigurované jako účet dbmanager nebo členem na vzdálený server Azure.

5.  Konfigurace databáze pro databázi roztáhnout, příkaz Vlastnosti databáze.

    1.  SERVER argument zadejte název existující Azure server, včetně `.database.windows.net` část jména \- například `MyStretchDatabaseServer.database.windows.net`.

    2.  Existující přihlašovacích údajů správce poskytnout argument pověření nebo je lze definovat FEDEROVANÝ\_služby\_účtu = zapnuto. Následující příklad uvádí existující přihlašovacích údajů.

    ```tsql
    ALTER DATABASE <database name>
        SET REMOTE_DATA_ARCHIVE = ON
            (
                SERVER = '<server_name>',
                CREDENTIAL = <db_scoped_credential_name>
            ) ;
    GO
    ```

## <a name="next-steps"></a>Další kroky
-   [Povolení roztáhnout databáze pro tabulku](sql-server-stretch-database-enable-table.md) povolit další tabulky.

-   [Sledování roztáhnout databáze](sql-server-stretch-database-monitor.md) se stav migrace dat.

-   [Pozastavit roztáhnout databáze](sql-server-stretch-database-pause.md)

-   [Správa a řešení potíží s roztáhnout databáze](sql-server-stretch-database-manage.md)

-   [Zálohování databáze roztáhnout s podporou](sql-server-stretch-database-backup.md)

## <a name="see-also"></a>Viz taky

[Určení databází a tabulek pro databázi roztáhnout](sql-server-stretch-database-identify-databases.md)

[Příkaz ALTER databáze nastavení možností (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx)

[FirewallRule]: ./media/sql-server-stretch-database-enable-database/firewall.png
