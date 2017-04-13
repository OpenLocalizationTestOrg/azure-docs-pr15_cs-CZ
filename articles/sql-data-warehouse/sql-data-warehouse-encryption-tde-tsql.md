<properties
   pageTitle="Šifrování průhledné dat v SQL datový sklad (T-SQL) | Microsoft Azure"
   description="Šifrování průhledné dat (TDE) v SQL datový sklad (T-SQL)"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/24/2016"
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="get-started-with-transparent-data-encryption-tde"></a>Začínáme s šifrování průhledné dat (TDE)


> [AZURE.SELECTOR]
- [Přehled zabezpečení](sql-data-warehouse-overview-manage-security.md)
- [Ověřování](sql-data-warehouse-authentication.md)
- [Šifrování (portál)](sql-data-warehouse-encryption-tde.md)
- [Šifrování (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## <a name="required-permssions"></a>Požadované oprávnění

Povolit šifrování průhledné dat (TDE), musíte být správce nebo členem dbmanager role.

## <a name="enabling-encryption"></a>Povolení šifrování

Postupujte podle těchto kroků můžete povolit TDE pro datový sklad SQL:

1. Připojení k databázi *předlohy* na hostitelském databázi přihlášení, které je správce nebo členem role **dbmanager** v hlavní databáze pomocí serveru
2. Spusťte následující příkaz šifrování databáze.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Zakázání šifrování

Tímto postupem zakázání TDE pro datový sklad SQL:

1. Připojení k databázi *předlohy* pomocí přihlášení, které je správce nebo členem role **dbmanager** v hlavní databázi
2. Spusťte následující příkaz šifrování databáze.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [AZURE.NOTE] Pozastavené SQL datový sklad musí obnovit před prováděním změn nastavení TDE.

## <a name="verifying-encryption"></a>Ověření šifrování

Zkontrolujte stav šifrování pro datový sklad SQL, postupujte následujícím způsobem:

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

## <a name="encryption-dmvs"></a>Šifrování DMVs  

- [Sys.Databases][] 
- [Sys.dm_pdw_nodes_database_encryption_keys][]


<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[Sys.Databases]: http://msdn.microsoft.com/library/ms178534.aspx  
[Sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
