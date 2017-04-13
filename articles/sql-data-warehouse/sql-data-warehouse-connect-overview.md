<properties
   pageTitle="Připojení k Azure SQL datový sklad | Microsoft Azure"
   description="Jak najít serveru název a připojovací řetězec pro vaše k datový sklad SQL Azure"
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
   ms.date="09/26/2016"
   ms.author="sonyama;barbkess"/>

# <a name="connect-to-azure-sql-data-warehouse"></a>Připojení k Azure SQL datový sklad

Tento článek pomáhá připojením k SQL datový sklad poprvé.

## <a name="find-your-server-name"></a>Zjištění názvu serveru

Prvním krokem při připojení k SQL datový sklad se, jak se zjištění názvu serveru.  Název serveru v následujícím příkladu je například sample.database.windows.net. Najít taky plně kvalifikovaný serveru název:

1. Přejděte na [portál Azure][].
2. Klikněte na příkaz **SQL databáze** 
3. Klikněte na databázi, kterou chcete připojit.
4. Vyhledejte úplný název serveru.

    ![Úplný název serveru][1]

## <a name="supported-drivers-and-connection-strings"></a>Podporované ovladače a připojovací řetězec

Azure SQL datový sklad podporuje [ADO.NET][], [ODBC][], [PHP][]a [JDBC][]. Klikněte na jednu z předchozího ovladače najdete nejnovější verzi, tak i si přečtěte následující dokumentaci. K automatickému vygenerování z portálu Microsoft Azure připojovací řetězec pro ovladač, který používáte, můžete kliknout na **Zobrazit řetězců připojení k databázi** z předchozího příkladu.  Jsou taky některé příklady vypadá připojovací řetězec pro každý ovladač.

> [AZURE.NOTE] Zvažte nastavení časového limitu připojení na připojení k zachován krátce nedostupnost 300 sekund.

### <a name="adonet-connection-string-example"></a>Příklad řetězce připojení ADO.NET

```C#
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

### <a name="odbc-connection-string-example"></a>Příklad řetězce připojení ODBC

```C#
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

### <a name="php-connection-string-example"></a>Příklad řetězce PHP připojení

```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

### <a name="jdbc-connection-string-example"></a>Příklad řetězce JDBC připojení

```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## <a name="connection-settings"></a>Nastavení připojení

SQL datový sklad standardizuje některá nastavení při připojení a vytvoření objektu. Tato nastavení nelze přepsat a patří:

| Nastavení databáze       | Hodnota                        |
| :--------------------- | :--------------------------- |
| [ANSI_NULLS][]         | ZAPNUTO                           |
| [QUOTED_IDENTIFIERS][] | ZAPNUTO                           |
| [FORMÁT_DATA][]         | MDY                          |
| [DATEFIRST][]          | 7                            |

## <a name="next-steps"></a>Další kroky

K připojení a dotaz s Visual Studiu, najdete v článku [dotaz s Visual Studio][]. Další informace o možnostech ověřování, najdete v článku [ověření datový sklad SQL Azure][].

<!--Articles-->
[Dotaz se Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[Ověřování datový sklad Azure SQL]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[ADO.NET]: https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx
[ROZHRANÍ ODBC]: https://msdn.microsoft.com/library/jj730314.aspx
[PHP]: https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396
[JDBC]: https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx
[ANSI_NULLS]: https://msdn.microsoft.com/library/ms188048.aspx
[QUOTED_IDENTIFIERS]: https://msdn.microsoft.com/library/ms174393.aspx
[FORMÁT_DATA]: https://msdn.microsoft.com/library/ms189491.aspx
[DATEFIRST]: https://msdn.microsoft.com/library/ms181598.aspx

<!--Other-->
[Azure portálu]: https://portal.azure.com

<!--Image references-->
[1]: media/sql-data-warehouse-connect-overview/get-server-name.png


