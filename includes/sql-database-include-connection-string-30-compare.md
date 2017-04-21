
<!--
includes/sql-database-include-connection-string-30-compare.md

Latest Freshness check:  2015-09-03 , GeneMi.

## Connection string
-->


### <a name="compare-the-connection-string"></a>Porovnání připojovacího řetězce


V následující tabulce porovná řetězce připojení, které aplikace C#, musí se připojit k místní SQL Server a databázi SQL Azure v cloudu. Rozdíly jsou tučným písmem.


| Připojovací řetězec pro<br/>Databáze Azure SQL | Připojovací řetězec pro<br/>Microsoft SQL Server |
| :-- | :-- |
| Server =**tcp:**{your_serverName_here}**. database.windows.net,1433**;<br/>ID uživatele = {your_loginName_here}**@{your_serverName_here}**;<br/>Heslo = {your_password_here};<br/>**Databáze = {your_databaseName_here};**<br/>**Časového limitu = 30**;<br/>**Šifrování = True**;<br/>**TrustServerCertificate = False**; | Server = {your_serverName_here};<br/>Uživatelské ID = {your_loginName_here};<br/>Heslo = {your_password_here}; |


**Databáze =** vynechán pro systém SQL Server, ale je nutné pro databáze SQL.


Tento článek popisuje [.NET ADO SqlConnectionStringBuilder vlastnosti](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder_properties.aspx) – všechny parametry podrobně.


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
