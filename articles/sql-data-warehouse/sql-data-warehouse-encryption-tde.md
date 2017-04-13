<properties
   pageTitle="Šifrování průhledné dat v SQL datový sklad (portál) | Microsoft Azure"
   description="Šifrování průhledné dat (TDE) v SQL datový sklad"
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

# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a>Začínáme s průhledné dat šifrování (TDE) v SQL datový sklad

> [AZURE.SELECTOR]
- [Přehled zabezpečení](sql-data-warehouse-overview-manage-security.md)
- [Ověřování](sql-data-warehouse-authentication.md)
- [Šifrování (portál)](sql-data-warehouse-encryption-tde.md)
- [Šifrování (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## <a name="required-permssions"></a>Požadované oprávnění

Povolit šifrování průhledné dat (TDE), musíte být správce nebo členem dbmanager role.

## <a name="enabling-encryption"></a>Povolení šifrování

Povolení TDE pro datový sklad SQL, postupujte následujícím způsobem:

1. Otevřete databázi v [Azure portálu](https://portal.azure.com)
2. V zásuvné databáze klikněte na tlačítko **Nastavení**
3. Vyberte možnost **průhledné šifrování**![][1]
4. Vyberte požadované nastavení **na**![][2]
5. Zaškrtněte políčko **Uložit**
![][3]  

## <a name="disabling-encryption"></a>Zakázání šifrování

Zakázání TDE pro datový sklad SQL, postupujte následujícím způsobem:

1. Otevřete databázi v [Azure portálu](https://portal.azure.com)
2. V zásuvné databáze klikněte na tlačítko **Nastavení**
3. Vyberte možnost **průhledné šifrování**![][1]
4. Vyberte požadované nastavení **Vypnuto**![][4]
5. Zaškrtněte políčko **Uložit**
![][5]  

## <a name="encryption-dmvs"></a>Šifrování DMVs

Pomocí následujících DMVs lze potvrdit šifrování:

- [Sys.Databases]
- [Sys.dm_pdw_nodes_database_encryption_keys]

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[Sys.Databases]: http://msdn.microsoft.com/library/ms178534.aspx
[Sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->
