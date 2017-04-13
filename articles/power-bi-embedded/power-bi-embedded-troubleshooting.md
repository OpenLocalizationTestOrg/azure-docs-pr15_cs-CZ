<properties
   pageTitle="Poradce při potížích s Microsoft Power BI vložený náhled"
   description="Poradce při potížích s Microsoft Power BI vložený náhled"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="microsoft-power-bi-embedded-preview-troubleshooting"></a>Poradce při potížích s Microsoft Power BI vložený náhled
V tomto článku najdete odpovědi pro řešení potíží s **Power BI vložený**.

<a name="connection-string"/>
## <a name="setting-sql-server-connection-strings"></a>Nastavení serveru SQL Server připojovací řetězec
Nastavení připojení řetězec SQL Server, budete muset podle určitého formátu. Tady je příklad připojovací řetězec pro systém SQL Server.

```
"Persist Security Info=False;Integrated Security=true;Initial Catalog=Northwind;server=(local)"
```

Další informace o serveru SQL Server připojovací řetězec, naleznete v následujících článcích:

-   [SQL Server připojovací řetězec](https://msdn.microsoft.com/library/jj653752.aspx)
-   [SqlConnection.ConnectionString](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.connectionstring.aspx)

<a name="credentials"/>
## <a name="setting-credentials"></a>Nastavení přihlašovacích údajů
V případě, kde máte přihlašovací údaje pro vývoj nebo pracovní prostředí, například uživatelské jméno a heslo může musíte aktualizovat přihlašovací údaje, které odpovídají řešení výroby.

## <a name="see-also"></a>Viz taky
- [Začínáme s ukázkovými](power-bi-embedded-get-started-sample.md)
- [Co je Power BI vložený](power-bi-embedded-what-is-power-bi-embedded.md)
