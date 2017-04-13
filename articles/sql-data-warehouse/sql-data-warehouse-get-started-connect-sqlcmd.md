<properties
   pageTitle="Dotaz SQL Azure datový sklad (sqlcmd) | Microsoft Azure"
   description="Dotazování datový sklad SQL Azure pomocí nástroje příkazového řádku sqlcmd."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/06/2016"
   ms.author="barbkess;sonyama"/>

# <a name="query-azure-sql-data-warehouse-sqlcmd"></a>Dotaz SQL Azure datový sklad (sqlcmd)

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Výukové Azure počítače](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [SqlCmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Tento návod používá nástroj příkazového řádku [sqlcmd][] k vytvoření dotazu datový sklad SQL Azure.  

## <a name="1-connect"></a>1. připojení

Začínáme s [sqlcmd][], otevřete příkazový řádek a zadejte **sqlcmd** následovaný připojovací řetězec pro databázi SQL datový sklad. Připojovací řetězec vyžaduje následujících parametrů:

+ **Serveru (-ne):** Server ve formě `<`název serveru`>`. database.windows.net
+ **Databáze (-d):** Název databáze.
+ **Povolit uvedená identifikátory (-můžu):** Identifikátory musí povolit připojení k SQL datový sklad instance.

Pokud chcete použít ověřování serveru SQL Server, potřebujete přidat parametry uživatelského jména a hesla:

+ **Uživatele (-U):** Uživatele serveru ve formuláři `<`uživatele`>`
+ **Heslo (-P):** Heslo přidružené k uživateli.

Připojovací řetězec může vypadat třeba takto:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

Pokud chcete použít Azure Active Directory ověřování, potřebujete přidat parametry Azure Active Directory:

+ **Ověřování služby azure Active Directory (-G):** pomocí služby Azure Active Directory pro ověřování

Připojovací řetězec může vypadat třeba takto:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [AZURE.NOTE] Budete muset [Povolit ověřování služby Azure Active Directory](sql-data-warehouse-authentication.md) ověření pomocí služby Active Directory.

## <a name="2-query"></a>2. dotaz

Po připojení můžete vydat všechny podporované příkazy jazyce Transact-SQL proti instance.  V tomto příkladu zadávání dotazů v interaktivní režimu.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Tyto další příklady ukazují, spouštění dotazů v režimu dávku pomocí možnosti – Q nebo potrubí SQL na sqlcmd.

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## <a name="next-steps"></a>Další kroky

Další informace o podrobnosti o dostupných možnostech na sqlcmd najdete v článku [si přečtěte následující dokumentaci sqlcmd][sqlcmd] .

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[SqlCmd]: https://msdn.microsoft.com/library/ms162773.aspx
[Azure portal]: https://portal.azure.com

<!--Other Web references-->
