<properties
   pageTitle="Povolit šifrování průhledné dat (TDE) pro roztáhnout databáze systému SQL Server na Azure TSQL | Microsoft Azure"
   description="Povolit šifrování průhledné dat (TDE) pro roztáhnout databáze systému SQL Server na Azure TSQL"
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
   ms.author="douglaslMS"/>

# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure-transact-sql"></a>Povolit šifrování průhledné dat (TDE) pro roztáhnout databáze na Azure (Transact-SQL)
> [AZURE.SELECTOR]
- [Azure portálu](sql-server-stretch-database-encryption-tde.md)
- [TSQL](sql-server-stretch-database-tde-tsql.md)

Šifrování průhledné dat (TDE) pomáhá chránit před hrozbou, že škodlivým aktivitám tak, že provedete v reálném čase šifrování a dešifrování databáze, přidružené zálohování a souborů protokolu transakce v klidu bez nutnosti změny aplikace.

TDE šifruje úložiště pro celou databázi pomocí symetrickou klíče s názvem šifrovací klíč databáze. Klíč šifrování databáze se po zamknutí tak, že certifikát předdefinované serveru. Certifikát předdefinované serveru je jedinečné pro každý Azure server. Microsoft automaticky: otočí tato poukázky minimálně každých 90 dní. Obecný popis TDE najdete v článku [Šifrování průhledné dat (TDE)].

##<a name="enabling-encryption"></a>Povolení šifrování

Povolení TDE pro Azure migrovat databázi, která je ukládání dat z databáze SQL serveru roztáhnout s podporou, proveďte následující akce:

1. Připojení k databázi *předlohy* na hostitelském databázi pomocí přihlášení, které je správce nebo členem role **dbmanager** v hlavní databázi Azure serveru
2. Spusťte následující příkaz šifrování databáze.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION ON;
```

##<a name="disabling-encryption"></a>Zakázání šifrování

Zakázání TDE pro Azure migrovat databázi, která je ukládání dat z databáze SQL serveru roztáhnout s podporou, proveďte následující akce:

1. Připojení k databázi *předlohy* pomocí přihlášení, které je správce nebo členem role **dbmanager** v hlavní databázi
2. Spusťte následující příkaz šifrování databáze.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION OFF;
```

##<a name="verifying-encryption"></a>Ověření šifrování

Pokud chcete ověřit, že stav šifrování databáze Azure, která je ukládání dat nemigruje z databáze SQL serveru podporou roztáhnout, proveďte následující akce:

1. Připojení k databázi *předlohy* nebo instance pomocí přihlášení, které je správce nebo členem role **dbmanager** v hlavní databázi
2. Spusťte následující příkaz šifrování databáze.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

Výsledek rovný ```1``` označuje šifrované databáze, ```0``` označuje bez šifrování databáze.


<!--Anchors-->
[Šifrování průhledné dat (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->

<!--Link references-->
