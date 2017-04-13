<properties
   pageTitle="Ověřování Azure SQL datový sklad | Microsoft Azure"
   description="Azure Active Directory (AAD) a SQL Server ověřování datový sklad SQL Azure."
   services="sql-data-warehouse"
   documentationCenter=""
   authors="byham"
   manager="barbkess"
   editor=""
   tags=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/24/2016"
   ms.author="rickbyh;barbkess;sonyama"/>

# <a name="authentication-to-azure-sql-data-warehouse"></a>Ověřování datový sklad Azure SQL

> [AZURE.SELECTOR]
- [Přehled zabezpečení](sql-data-warehouse-overview-manage-security.md)
- [Ověřování](sql-data-warehouse-authentication.md)
- [Šifrování (portál)](sql-data-warehouse-encryption-tde.md)
- [Šifrování (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

Připojení k SQL datový sklad, musí uplynout v zabezpečovací přihlašovací údaje pro účely ověření. Po vytvoření připojení, některé připojení nakonfigurováno v rámci zřízení relaci dotazu.  

Další informace o zabezpečení a jak povolit připojení k datový sklad najdete v článku [zabezpečení databáze systému SQL datový sklad][].

## <a name="sql-authentication"></a>Ověřování serveru SQL
Připojení k SQL datový sklad, je nutné zadat následující informace:

- Plně kvalifikovaný webu název serveru
- Určení ověřování serveru SQL
- Uživatelské jméno
- Heslo
- Výchozí databáze (volitelné)

Ve výchozím nastavení připojení ke *hlavní* databáze a ne databázi uživatele. Připojení k databázi uživateli, můžete udělat jednu z těchto akcí:

- Zadejte výchozí databázi při registraci serveru SQL Server Object Explorer SSDT SSMS, nebo v připojovacím řetězci aplikace. Například InitialCatalog parametr připojení ODBC.
- Zvýraznění databázi uživatele před vytvořením relace v SSDT.

> [AZURE.NOTE] Příkaz Transact-SQL **Databáze použití;** nepodporuje ke změně databáze pro připojení. Pokyny pro připojení k SQL datový sklad s SSDT naleznete v článku [dotaz s Visual Studio][] .

## <a name="azure-active-directory-aad-authentication"></a>Ověřování Azure Active Directory (AAD)

[Azure Active Directory] [ What is Azure Active Directory] ověřování je mechanismus připojení k Microsoft Azure SQL datový sklad pomocí identit v Azure Active Directory (Azure AD). Ověřování služby Azure Active Directory centrální Správa identit uživatelů databáze a další služby od Microsoftu na jednom centrálním místě. Centrální správa ID obsahuje mít na jednom místě ke správě SQL datový sklad uživatelů a zjednodušuje správy oprávnění. 

### <a name="benefits"></a>Výhody

Azure Active Directory výhody patří:

- Poskytuje alternativy ověřování serveru SQL Server.
- Pomáhá zastavit šíření identity uživatele na serverech databáze.
- Umožňuje heslo otočení na jednom místě
- Správa oprávnění k databázi pomocí vnější skupiny (AAD).
- Ukládání hesel eliminuje povolením integrované ověřování systému Windows a jinými formami ověřování podporovaných službou Azure Active Directory.
- Použití obsažené databázi uživatelům ověření identity na úrovni databáze.
- Podporuje token ověřování pro připojení k SQL datový sklad aplikace.
- Podporuje Vícefaktorové ověřování prostřednictvím univerzální ověřování služby Active Directory pro SQL Server Management Studio. Popis Vícefaktorové ověřování najdete v tématu [SSMS podpory pro MFA Azure AD pomocí databáze SQL a SQL datový sklad](../sql-database/sql-database-ssms-mfa-authentication.md).

> [AZURE.NOTE] Azure Active Directory je pořád relativně nový a má určitá omezení. Zajistěte, aby byl Azure Active Directory přesně hodí prostředí, najdete v článku [Azure AD funkce a omezení][], konkrétně dalších aspektech.

### <a name="configuration-steps"></a>Kroky konfigurace

Tyto kroky konfigurace ověřování služby Azure Active Directory.

1. Vytvoření a naplnění Azure Active Directory
2. Volitelné: Přidružení nebo změna active directory, která je aktuálně přidružený k předplatnému Azure
3. Vytvoření správce služby Azure Active Directory pro datový sklad SQL Azure.
4. Konfigurace klientské počítače
5. Vytvoření uživatelů obsažené databáze v databázi namapované Azure AD identity
6. Připojení k datový sklad pomocí Azure AD identity

Uživatelům Azure Active Directory nejsou aktuálně zobrazený v prohlížeči SSDT objektů. Jako alternativu zobrazte uživatele [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).
  
### <a name="find-the-details"></a>Podrobné informace
- Postupujte podle podrobných pokynů. Databáze SQL Azure a Azure SQL datový sklad téměř shodná podrobně popisuje, jak konfigurovat a používat ověřování služby Azure Active Directory. Podrobný postup v tématu [připojení k databázi SQL nebo SQL datový sklad tak, že pomocí Azure Active Directory, ověřování](../sql-database/sql-database-aad-authentication.md).
- Vytvoření vlastní databáze rolí a přidání uživatelů do role. Potom oprávnění podrobného k rolím. Další informace najdete v tématu [Začínáme s oprávněními Engine databáze](https://msdn.microsoft.com/library/mt667986.aspx).

## <a name="next-steps"></a>Další kroky

Dotazování datový sklad s Visual Studia a jiných aplikacích zahájíte najdete v článku [dotaz s Visual Studio][].

<!-- Article references -->
[Zabezpečení databáze aplikace SQL datový sklad]: ./sql-data-warehouse-overview-manage-security.md
[Dotaz se Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[What is Azure Active Directory]: ../active-directory/active-directory-whatis.md
[Azure AD funkce a omezení]: ../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations
