<properties
    pageTitle="Začínáme s více databázových dotazů (svislá dělení) | Microsoft Azure"   
    description="jak pomocí svisle rozdělený databází pružná databázových dotazů"
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="torsteng"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="torsteng" />

# <a name="get-started-with-cross-database-queries-vertical-partitioning-preview"></a>Začínáme s více databázových dotazů (svislá dělení) (verze preview)

Pružná databázových dotazů (verze preview) pro databázi SQL Azure umožňuje spouštění dotazů T-SQL, které zahrnují více databázím pomocí jednoho spojovací bod. Toto téma se týká [svisle oddíly databází](sql-database-elastic-query-vertical-partitioning.md).  

Po dokončení, udělejte toto: Přečtěte si, jak konfigurovat a používat databázi SQL Azure k provádění dotazů, které zahrnují více související databází. 

Další informace o funkci dotazu pružná databáze najdete [Přehled dotazů pružná databáze databáze SQL Azure](sql-database-elastic-query-overview.md). 

## <a name="create-the-sample-databases"></a>Vytvoření ukázkové databáze

Abyste mohli začít potřebujeme vytvořit dvě databáze, které **Zákazníci** a **objednávky**, buď ve stejném nebo jiném logických serverů.   

Proveďte následující dotazy na databázi **objednávky** a vytvořte v tabulce **OrderInformation** ukázkových dat pro zadávání. 

    CREATE TABLE [dbo].[OrderInformation]( 
        [OrderID] [int] NOT NULL, 
        [CustomerID] [int] NOT NULL 
        ) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8) 

Teď provedení sledování dotazu v databázi **Zákazníci** a vytvořte v tabulce **CustomerInformation** ukázkových dat pro zadávání. 

    CREATE TABLE [dbo].[CustomerInformation]( 
        [CustomerID] [int] NOT NULL, 
        [CustomerName] [varchar](50) NULL, 
        [Company] [varchar](50) NULL 
        CONSTRAINT [CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC) 
    ) 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (1, 'Jack', 'ABC') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (2, 'Steve', 'XYZ') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (3, 'Lylla', 'MNO') 

## <a name="create-database-objects"></a>Vytvoření databázových objektů
### <a name="database-scoped-master-key-and-credentials"></a>Databáze omezené hlavní klíč a přihlašovací údaje

1. Otevřete SQL Server Management Studio nebo SQL Server Data Tools ve Visual Studiu.
2. Připojení k databázi objednávky a následující příkazy T-SQL:

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>'; 
        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred 
        WITH IDENTITY = '<username>', 
        SECRET = '<password>';  

    "Uživatelské jméno" a "heslo" by měl být uživatelské jméno a heslo k přihlášení do databáze zákazníky.
    Ověření pomocí služby Azure Active Directory s pružná dotazů není aktuálně podporován.

### <a name="external-data-sources"></a>Externí zdroje dat
Pokud chcete vytvořit externí zdroj dat, spusťte následující příkaz objednávky pro databázi: 

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH 
        (TYPE = RDBMS, 
        LOCATION = '<server_name>.database.windows.net', 
        DATABASE_NAME = 'Customers', 
        CREDENTIAL = ElasticDBQueryCred, 
    ) ;

### <a name="external-tables"></a>Externí tabulky
Vytvořte externí tabulku objednávky databázi, která odpovídá definici CustomerInformation tabulky:

    CREATE EXTERNAL TABLE [dbo].[CustomerInformation] 
    ( [CustomerID] [int] NOT NULL, 
      [CustomerName] [varchar](50) NOT NULL, 
      [Company] [varchar](50) NOT NULL) 
    WITH 
    ( DATA_SOURCE = MyElasticDBQueryDataSrc) 

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Spuštění dotazu T-SQL pružná databáze ukázkové

Pokud jste definovali externího zdroje dat a externí tabulkách teď můžete T-SQL k vytvoření dotazu externí tabulkách. Provedení tohoto dotazu na databázi objednávky: 

    SELECT OrderInformation.CustomerID, OrderInformation.OrderId, CustomerInformation.CustomerName, CustomerInformation.Company 
    FROM OrderInformation 
    INNER JOIN CustomerInformation 
    ON CustomerInformation.CustomerID = OrderInformation.CustomerID 

## <a name="cost"></a>Pole náklady

Funkce dotazu pružná databáze je v současné době zahrnuta do náklady databázi SQL Azure.  

Ceny informace najdete v článku [Ceny databáze SQL](/pricing/details/sql-database). 


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->

<!--anchors-->
