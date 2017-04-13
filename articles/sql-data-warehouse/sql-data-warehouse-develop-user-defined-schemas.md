<properties
   pageTitle="Uživatelem definované schémat v SQL datový sklad | Microsoft Azure"
   description="Tipy pro použití jazyce Transact-SQL schémat v SQL Azure datový sklad k vývoji řešení."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="user-defined-schemas-in-sql-data-warehouse"></a>Uživatelem definované schémat v SQL datový sklad

Sklady tradiční dat k vytvoření aplikace omezení na základě zátěží na projektu, domény nebo zabezpečení často použít samostatném databází. Například tradiční SQL Server datový sklad můžou obsahovat pracovní databázi, databázi datový sklad a některé databáze Tržiště data. V této topologii každou databázi funguje jako pracovního vytížení a hranice zabezpečení v architektuře.

Naopak SQL datový sklad spustí pracovní zátěž celý datový sklad v rámci jedné databázové. Křížové databáze nejsou povoleny spojení. Proto SQL datový sklad předpokládá, že všechny tabulky pomocí sklad být uložena v jedné databáze.

> [AZURE.NOTE] SQL datový sklad nepodporuje křížového databázových dotazů k žádnému. Proto implementace sklad dat, které využít tento způsob muset zkontrolovali.

## <a name="recommendations"></a>Doporučení

Toto jsou doporučení pro sloučení zatížení, zabezpečení, domény a funkční hranice pomocí schémat definované uživatelem

1. Použití jedné databáze SQL datový sklad pro spuštění pracovního vytížení vaší celý datový sklad
2. Sloučení sklad prostředí existující data použít jednu databázi SQL datový sklad
3. Využít **uživatelem definovaných schémata** poskytnout hranici dříve implementovaná databází.

Pokud nepoužili uživatelem definovaných schémata dříve máte mít čistý stůl. Jednoduše použijte původní název databáze jako základ pro váš uživatelský schémata v databázi SQL datový sklad.

Pokud schémata jste již používali máte několik možností:

1. Odebrat názvy starší verze schématu a začít pracovat
2. Zachovat názvy starší verze schématu tak, že před komentář přidejte název starší verze schématu k názvu tabulky
3. Zachovat názvy starší verze schématu implementací zobrazení v tabulce v schématem navíc k opětovnému vytvoření staré konstrukce schématu.

> [AZURE.NOTE] Na první kontrolu možnost 3 zdánlivě jako možnost nejčastěji bude profesionálně vypadat. Ďábla je však k podrobnostem. Zobrazení se budou číst jenom v SQL datový sklad. Jakékoliv úpravy dat nebo by potřeba provést proti základní tabulka. Možnost 3 zavádí vrstvě zobrazení do vašeho systému. Můžete se dát to některé další myšlenkových, pokud používáte zobrazení ve vaší architektuře už.


### <a name="examples"></a>Příklady:

Implementace uživatelem definovaných schémata podle názvů databáze

```sql
CREATE SCHEMA [stg]; -- stg previously database name for staging database
GO
CREATE SCHEMA [edw]; -- edw previously database name for the data warehouse
GO
CREATE TABLE [stg].[customer] -- create staging tables in the stg schema
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[customer] -- create data warehouse tables in the edw schema
(       CustKey BIGINT NOT NULL
,       ...
);
```

Ponechat názvy starší verze schématu tak, že před komentář přidejte, na název tabulky. Schémata určenou hranici zátěží na projektu.

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- edw defines the data warehouse boundary
GO
CREATE TABLE [stg].[dim_customer] --pre-pend the old schema name to the table and create in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[dim_customer] --pre-pend the old schema name to the table and create in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
);
```

Zachovat starší verze schématu názvy pomocí zobrazení

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- stg defines the data warehouse boundary
GO
CREATE SCHEMA [dim]; -- edw defines the legacy schema name boundary
GO
CREATE TABLE [stg].[customer] -- create the base staging tables in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
)
GO
CREATE TABLE [edw].[customer] -- create the base data warehouse tables in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
)
GO
CREATE VIEW [dim].[customer] -- create a view in the legacy schema name boundary for presentation consistency purposes only
AS
SELECT  CustKey
,       ...
FROM    [edw].customer
;
```

> [AZURE.NOTE] Jakákoli změna schématu strategie musí seznámit s model zabezpečení databáze. V mnoha případech je možné zjednodušit model zabezpečení přiřazením oprávnění na úrovni schéma. Pokud je podrobnějších oprávnění jsou potřeba můžete databázi role.

## <a name="next-steps"></a>Další kroky
Další tipy vývoj najdete v tématu [Přehled vývoje][].

<!--Image references-->

<!--Article references-->
[Přehled vývoje]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
