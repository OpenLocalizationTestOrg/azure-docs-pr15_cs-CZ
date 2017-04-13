<properties
    pageTitle="Kódy chyb SQL - Chyba připojení databáze | Microsoft Azure"
    description="Seznamte se s kódy chyb SQL pro klientské aplikace databáze SQL, jako jsou běžné chyby při připojení databáze, problémy kopii databáze a obecné chyby. "
    keywords="Kód chyby SQL aplikace access sql, Chyba připojení databáze, kódy chyb sql"
    services="sql-database"
    documentationCenter=""
    authors="annemill"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="annemill"/>


# <a name="sql-error-codes-for-sql-database-client-applications-database-connection-error-and-other-issues"></a>Kódy chyb SQL pro databáze SQL klientských aplikacích: databáze Chyba připojení i jiných problémů


<!--
Old Title on MSDN:  Error Messages (Azure SQL Database)
ShortId on MSDN:  ff394106.aspx
Dx 4cff491e-9359-4454-bd7c-fb72c4c452ca
-->


Tento článek uvádí kódy chyb SQL databáze SQL klientských aplikací, včetně chyby připojení databáze přechodná chyby (nazývané také přechodná chyby), chyby zásady správného řízení zdrojů, problémy kopii databáze, pružná fondu a jiné chyby. Většina kategorie odpovídají databáze SQL Azure a nebudou mít vliv na Microsoft SQL Server.

## <a name="database-connection-errors-transient-errors-and-other-temporary-errors"></a>Chyby připojení databáze, přechodná chyb a jiné dočasné chyby

Následující tabulka popisuje kódy chyb SQL chyby ztrátě připojení a jiné přechodná chyby, ke kterým může dojít při aplikace pokusí o přístup k databázi SQL. Zobrazuje Začínáme výukové programy pro připojení k databázi SQL Azure, najdete v tématu [připojení k databázi SQL Azure](sql-database-libraries.md).

### <a name="most-common-database-connection-errors-and-transient-fault-errors"></a>Nejběžnější chybám připojení databáze a přechodná poruch

Azure infrastruktury má dynamické upravit servery při hodně úloh nastat ve službě databáze SQL.  Toto chování dynamické může způsobit klientským programem ztratit připojení k databázi SQL. Tento typ chybovém stavu nazývá *přechodná poruch*.

Pokud klientským programem Logika opakování, můžete zkusit o obnovení připojení po pojmenování čas přechodná poruch opravě slova.  Doporučujeme zpoždění 5 sekund před prvním opakovat. Opakování zpožděním kratší než 5 sekund rizika neunavily cloudovou službu. Pro každý další pokusy zpoždění růst geometrickou řadou až do velikosti 60 sekund.

Přechodné poruch chyby obvykle projevovat jako jednu z následujících chybových zpráv z klientských programů:

- Databáze < db_name > na serveru < Azure_instance > není momentálně neexistuje. Opakujte připojení později. Pokud problém přetrvává, kontaktujte zákaznickou podporu a předejte mu ID trasování relace < session_id >

- Databáze < db_name > na serveru < Azure_instance > není momentálně neexistuje. Opakujte připojení později. Pokud problém přetrvává, kontaktujte zákaznickou podporu a předejte mu ID trasování relace < session_id >. (Microsoft SQL Server, chyba: 40613)

- Existující připojení byl vynutit ukončená vzdáleného hostitele.

- System.Data.Entity.Core.EntityCommandExecutionException: Došlo k chybě při provádění příkazu definice. Viz vnitřní výjimka podrobnosti. ---> System.Data.SqlClient.SqlException: úrovni přenosu došlo k chybě při příjmu výsledků ze serveru. (poskytovatel: zprostředkovatele relací, chyba: 19 - fyzické připojení není)

Příklady použití logických operátorů opakovat najdete v článku:

- [Knihovny připojení databáze SQL a SQL Server](sql-database-libraries.md) 
- [Akce, které mají oprava chyb připojení a přechodná chyb v databázi SQL](sql-database-connectivity-issues.md)

Informace o *blokování období* pro klienty, kteří používají ADO.NET je k dispozici v [SQL Server připojení fond (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

### <a name="transient-fault-error-codes"></a>Kódy chyb přechodná poruch

Následující chyby jsou přechodné a má opakovat v aplikaci logiky 

| Kód chyby | Závažnosti | Popis |
| ---: | ---: | :--- |
| 4060 | 16 | Databázi nelze otevřít "% & #x2a; ls" požadoval přihlášení. Přihlášení se nezdařila. |
|40197|17|Služba došlo k chybě zpracování požadavku. Zkuste to znovu. Kód chyby %d.<br/><br/>Tato chyba, dostanou po službu dolů kvůli software nebo upgrady hardwaru, selhání hardwaru nebo jiné problémy překlopení. Kód chyby (%d) vložený do zprávy chyby 40197 poskytující další informace o typu selhání nebo převzetí výskytu. Několik příkladů chyby do schránky, které kódy jsou vložené do zprávy chyby 40197 jsou 40020, 40143, 40166 a 40540.<br/><br/>Opětovné připojení k databázi SQL serveru automaticky připojit se k správný kopii databáze. Vaše aplikace musí zachytit 40197, protokolu chyb kód vložený chyby (%d) v rámci zprávy pro řešení potíží a zkuste se znovu připojit k databázi SQL tak, aby zdroje jsou k dispozici a vaše připojení znovu.|
|40501|20|Služba je nyní zaneprázdněn. Opakování žádosti po 10 sekund. Vyřešení incidentu ID: %ls. Kód: %d.<br/><br/>*Poznámka:* Další informace najdete v tématu:<br/>• [Omezení zdrojů databáze SQL azure](sql-database-resource-limits.md).
|40613|17|Databáze "% & #x2a; ls" na serveru "% & #x2a; ls" není momentálně neexistuje. Opakujte připojení později. Pokud problém přetrvává, kontaktujte zákaznickou podporu a předejte mu ID relace, trasování "% & #x2a; ls".|
|49918|16|Žádost o se nedají zpracovat. Není dost prostředků pro zpracování požadavku.<br/><br/>Služba je nyní zaneprázdněn. Opakujte žádost později. |
|49919|16|Nelze proces vytvoření nebo aktualizace žádost. Příliš mnoho vytvoření nebo aktualizace průběhu operace pro předplatné "% ld".<br/><br/>Teď na něčem pracuje služba zpracování více vytvoření nebo aktualizace žádosti o předplatného nebo serveru. Požadavky jsou aktuálně blokovány pro optimalizaci zdroje. Dotaz [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) pro operace čekající. Počkejte, dokud čekající na na Vytvořit nebo aktualizovat žádosti o dokončení nebo odstranit jednoho z vašich požadavků na zjištění stavu úkolů a zopakujte požadavek později. |
|49920|16|Žádost o se nedají zpracovat. Příliš mnoho operace probíhá pro předplatné "% ld".<br/><br/>Služba je zaneprázdněné, zpracování více požadavků pro toto předplatné. Požadavky jsou aktuálně blokovány pro optimalizaci zdroje. Dotaz [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) pro stav operace. Počkejte, dokud čeká na vyřízení žádosti o dokončení nebo odstranit jednoho z vašich požadavků na zjištění stavu úkolů a opakování žádosti o později. |

## <a name="database-copy-errors"></a>Chyby kopie databáze

Následující chyby může dojít při kopírování databáze v databázi SQL Azure. Další informace najdete v tématu [kopírování databáze SQL Azure](sql-database-copy.md).


|Kód chyby|Závažnosti|Popis|
|---:|---:|:---|
|40635|16|Klient s IP adresu "% & #x2a; ls" dočasně zakázán.|
|40637|16|Vytvoření kopie databáze právě zakázané.|
|40561|16|Kopie databáze se nezdařila. Zdrojový nebo cílový databáze neexistuje.|
|40562|16|Kopie databáze se nezdařila. Zdrojové databáze bylo zrušeno.|
|40563|16|Kopie databáze se nezdařila. Cílová databáze obsahuje nezobrazí.|
|40564|16|Kopírovat došlo k interní chybě databáze. Uvolnění databáze cílové a zkuste to znovu.|
|40565|16|Kopie databáze se nezdařila. Víc než 1 kopie souběžné databáze ze stejného zdroje jsou povolené. Uvolnění databáze cílové a zkuste to později.|
|40566|16|Kopírovat došlo k interní chybě databáze. Uvolnění databáze cílové a zkuste to znovu.|
|40567|16|Kopírovat došlo k interní chybě databáze. Uvolnění databáze cílové a zkuste to znovu.|
|40568|16|Kopie databáze se nezdařila. Zdrojové databáze je k dispozici. Uvolnění databáze cílové a zkuste to znovu.|
|40569|16|Kopie databáze se nezdařila. Cílová databáze obsahuje nedostupné. Uvolnění databáze cílové a zkuste to znovu.|
|40570|16|Kopírovat došlo k interní chybě databáze. Uvolnění databáze cílové a zkuste to později.|
|40571|16|Kopírovat došlo k interní chybě databáze. Uvolnění databáze cílové a zkuste to později.|

## <a name="resource-governance-errors"></a>Chyby zásady správného řízení zdrojů

Následující chyby příčinou nadbytečné využití prostředků při práci s databáze SQL Azure. Příklad:

- Transakce byl otevřít moc dlouho.
- Transakce je příliš mnoho zámky.
- Aplikace je využití příliš mnoho paměti.
- Aplikaci spotřebovává příliš `TempDb` místa.

Související témata:

* Podrobnější informace najdete tady: [omezení zdrojů databáze SQL Azure](sql-database-resource-limits.md)

|Kód chyby|Závažnosti|Popis|
|---:|---:|:---|
|10928|20|Pole číslo ID zdroje: %d. Limit %s databáze je a překročení. Další informace najdete v tématu [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637).<br/><br/>Číslo ID zdroje označuje, prostředek, který má limitu. Pro pracovní vlákna číslo ID zdroje = 1. Pro relace, číslo ID zdroje = 2.<br/><br/>*Poznámka:* Další informace o této chybě a jak ji řešit najdete tady:<br/>• [Omezení zdrojů databáze SQL azure](sql-database-resource-limits.md). |
|10929|20|Pole číslo ID zdroje: %d. Minimální záruky %s je %d, je maximální limit a aktuální použití databáze je %d. Server je však aktuálně přeplněné podporuje větší než %d požadavky pro tuto databázi. Další informace najdete v tématu [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637). Jinak zkuste znovu později.<br/><br/>Číslo ID zdroje označuje, prostředek, který má limitu. Pro pracovní vlákna číslo ID zdroje = 1. Pro relace, číslo ID zdroje = 2.<br/><br/>*Poznámka:* Další informace o této chybě a jak ji řešit najdete tady:<br/>• [Omezení zdrojů databáze SQL azure](sql-database-resource-limits.md).|
|40544|20|Databáze dosáhla kvóta velikost. Oddílu nebo odstranění dat, přetažení indexy nebo možná řešení v dokumentaci.|
|40549|16|Relace je ukončen, protože máte dlouho probíhajících transakce. Zkuste zkrácení transakce.|
|40550|16|Protože získal příliš mnoho zámky byl ukončen relace. Zkuste čtení nebo úprava menší počet řádků v jedné transakci.|
|40551|16|Relace byl ukončen z důvodu nadbytečné `TEMPDB` použití. Pokuste se změnit dotaz zmenšit místo použití dočasné tabulky.<br/><br/>*Tip:* Pokud používáte dočasné objekty, ušetřit místo v `TEMPDB` databáze umístěním dočasné objekty, po které už nepotřebujete relace.|
|40552|16|Z důvodu nadbytečné transakční protokol místo použití byl ukončen relace. Pokuste se změnit menší počet řádků v jedné transakci.<br/><br/>*Tip:* Pokud provedete hromadné vloží pomocí `bcp.exe` nástroj nebo `System.Data.SqlClient.SqlBulkCopy` třídy, zkuste použít `-b batchsize` nebo `BatchSize` možnosti omezit počet řádků zkopírují do serveru v každou transakci. Pokud jsou opětovné vytvoření indexu s `ALTER INDEX` použití příkazu funkce, zkuste použít `REBUILD WITH ONLINE = ON` možnost.|
|40553|16|Z důvodu spotřebu paměti nadbytečné byl ukončen relace. Pokuste se změnit dotaz zpracuje menší počet řádků.<br/><br/>*Tip:* Omezení počtu `ORDER BY` a `GROUP BY` operace v jazyce Transact-SQL kódu snižuje požadavky na paměti dotazu.|

## <a name="elastic-pool-errors"></a>Pružná fondu chyby

Následující chyby jsou týkající se vytváření a používání Elastics fondů.

| Argument číslo chyby | ErrorSeverity | ErrorFormat | ErrorInserts | ErrorCause | ErrorCorrectiveAction |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 1132 | EX_RESOURCE | Pružná fondu dosáhla limitu úložiště. Využití úložiště fondu pružná nesmí překročit (%d) MB. | Pružná fondu omezení místa v MB. | Pokus o při dosáhl limit úložiště pružné fond zápis dat do databáze. | Zvažte zvýšení DTUs Pokud je to možné fondu pružná zvětšit limitu úložiště, snížit použité jednotlivé databází v rámci pružná fondu úložiště nebo odebrání databáze z fondu pružná. |
| 10929 | EX_USER | Minimální záruky %s je %d, je maximální limit a aktuální použití databáze je %d. Server je však aktuálně přeplněné podporuje větší než %d požadavky pro tuto databázi. V tématu [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637) požádejte o pomoc. Jinak zkuste znovu později. | Min DTU na databázi. Max DTU podle databáze | Celkový počet souběžné pracující s informacemi (požadavky) přes všechny databáze ve fondu pružné pokus o přesáhne limit fond. | Zvažte zvýšení DTUs Pokud je to možné fondu pružná zvýšit limit pracovní nebo odebrání databáze z fondu pružná. |
| 40844 | EX_USER | Databáze %ls na serveru %ls databázi %ls edition ve fondu pružná a nesmí obsahovat nepřetržitý kopírovat relace. | Název databáze, databáze vydání, název serveru | Příkaz StartDatabaseCopy vydává db není premium do fondu pružná. | Již brzy |
| 40857 | EX_USER | Pružná fondu nebyl nalezen pro server: %ls, název pružná fondu: %ls. | název serveru. Název pružná fondu | Zadaný pružná fond neexistuje v zadaném serveru. | Zadejte název platné pružná fondu. |
| 40858 | EX_USER | Pružná fond %ls již existuje na serveru: %ls | Pružná fondu název, název serveru | Zadaný pružná fond již existuje v zadaném logické server. | Zadejte název nového pružná fondu. |
| 40859 | EX_USER | Pružná fondu nepodporuje vrstvy služeb %ls. | Pružná fondu vrstvy služeb | Vrstvy zadaný služeb není podporována pro zřizování pružná fondu. | Zadejte správné edition nebo nechejte prázdné použít výchozí vrstvu služby vrstvy služeb. |
| 40860 | EX_USER | Pružná fondu %ls a přihlašovacích údajů cíle %ls kombinace není platná. | Název pružné fondu; Název cíle úrovně služeb | Pružná fondu a přihlašovacích údajů cíle můžete zadat společně jenom v případě, že služba cílem není zadán jako "ElasticPool". | Zadejte správné kombinace pružná fondu a služby cíle. |
| 40861 | EX_USER | Databáze edition "%. *ls' nelze se liší od vrstvy pružná fondu služeb, které je "%.* ls účastníků. | databáze edition vrstvy pružné fond služeb | Edition databáze se liší od osy pružné fond služby. | Zadejte není edition databáze, které se liší od osy pružná fondu služby.  Všimněte si, že edition databáze nemusí být zadán. |
| 40862 | EX_USER | Pružná fondu název musí být zadaný, pokud není zadán cíle pružná fondu služby. | Žádná | Pružná fondu služby cílem není jednoznačně pružná fondu. | Zadejte název pružná fondu používáte cíle pružná fondu služby. |
| 40864 | EX_USER | DTUs pružná fondu musí být alespoň (%d) DTUs vrstvy služeb "%. * ls'. | DTUs pružná fondu; vrstvy pružné fond služeb. | Pokus o nastavení DTUs pružná fondu nižší než minimální omezení. | Opakujte nastavení DTUs ohebné fondu aspoň minimální limit. |
| 40865 | EX_USER | DTUs pružná fondu nesmí překročit (%d) DTUs vrstvy služeb "%. * ls'. | DTUs pružná fondu; vrstvy pružné fond služeb. | Pokus o nastavení DTUs pružné fond nad maximální limit. | Opakujte nastavení DTUs pružná fondu větší než mezní hodnota. |
| 40867 | EX_USER | Maximální počet databáze DTU musí být aspoň (%d) pro službu osy "%. * ls'. | Max DTU na databázi. Pružná fondu vrstvy služeb | Pokus o nastavení max DTU na databázi pod podporovaný. | Zvažte použití vrstvy pružná fondu služeb, který podporuje na požadované nastavení. |
| 40868 | EX_USER | Maximální počet databáze DTU nesmí překročit (%d) pro službu osy "%. * ls'. | Max DTU na databázi. Pružná fondu vrstvy služeb. | Pokus o nastavení max DTU na databázi mimo podporovaný. | Zvažte použití vrstvy pružná fondu služeb, který podporuje na požadované nastavení. |
| 40870 | EX_USER | Min DTU na databázi nesmí překročit (%d) pro službu osy "%. * ls'. | Min DTU na databázi. Pružná fondu vrstvy služeb. | Pokus o nastavení min DTU na databázi mimo podporovaný. | Zvažte použití vrstvy pružná fondu služeb, který podporuje na požadované nastavení. |
| 40873 | EX_USER | Počet databáze (%d) a DTU min za databázi (%) nesmí překročit DTUs fondu pružná (%d). | Číslo databází v pružná fondu; Min DTU na databázi. DTUs pružná fondu. | Pokus o zadání DTU min pro databáze do fondu pružná překračující DTUs fondu pružná. | Zvažte zvýšení DTUs fondu pružná nebo zmenšit min DTU na databázi nebo snížení počtu databází ve fondu pružná. |
| 40877 | EX_USER | Pružná fondu nelze odstranit, pokud neobsahuje žádné databáze. | Žádná | Pružná fond obsahuje jednu nebo více databází a proto se nedají odstranit. | Pokud chcete odstranit odeberte databáze z fondu pružná. |
| 40881 | EX_USER | Pružná fondu "%. * ls' dosáhla limitu počet databáze.  Limit počtu databáze pružná fondu nesmí překročit (%d) pružná fondu s (%d) DTUs. | Název pružná fondu; limit počtu databáze pružná fondu; e DTUs fond zdrojů. | Pokus o vytvoření nebo přidání databáze do pružná fondu dosáhne limit počtu databáze pružná fondu. | Zvažte zvýšení DTUs Pokud je to možné fondu pružná zvýšit jeho limitu nebo odebrání databáze z fondu pružná. |
| 40889 | EX_USER | DTUs nebo limit úložiště pružná fondu "%. * ls' nelze zmenšená, od které by poskytují dostatek prostoru úložiště pro své databáze. | Název pružná fondu. | Pokus o zmenšení limit úložiště fondu pružná pod využití úložiště. | Zmenšete využití úložiště jednotlivých databází ve fondu pružná nebo odebrat databáze z fondu a zmenšení její DTUs nebo limit úložiště. |
| 40891 | EX_USER | Min DTU za databázi (%) nesmí překročit max DTU za databázi (%). | Min DTU na databázi. DTU maximální počet databáze. | Pokus o nastavení min DTU na databázi vyšší než maximální DTU na databázi. | Zkontrolujte, zda že min DTU za databází nepřekročí max DTU na databázi. |
| TBD | EX_USER | Velikost úložiště pro jednotlivé databáze ve pružná fondu nesmí překročit maximální velikosti povolené na základě "%. * ls' služby pružná fondu osy. | Pružná fondu vrstvy služeb | Maximální velikost databáze větší než maximální velikosti povolené na základě vrstvy pružná fondu služeb. | Nastavte maximální velikost databáze v určených mezích maximální velikosti povolené na základě vrstvy pružná fondu služeb. |

Související témata:

* [Vytvoření fondu pružné databáze (C#)](sql-database-elastic-pool-create-csharp.md) 
* [Spravovat fondu pružná databáze (C#)](sql-database-elastic-pool-manage-csharp.md). 
* [Vytvoření fondu pružná databáze (Powershellu)](sql-database-elastic-pool-create-powershell.md) 
* [Sledování a správa fondu pružná databáze (Powershellu)](sql-database-elastic-pool-manage-powershell.md).

## <a name="general-errors"></a>Obecné chyby

Následující chyby nepatří do předchozí kategorií.

|Kód chyby|Závažnosti|Popis|
|---:|---:|:---|
|15006|16|<AdministratorLogin>není platný název protože obsahuje neplatné znaky.|
|18452|14|Přihlaste se nezdařila. Přihlášení z nedůvěryhodných domény a nemohou být použity s Windows authentication.% & #x2a; ls (Windows přihlášení nejsou podporované v této verzi systému SQL Server).|
|18456|14|Přihlášení uživatele se nezdařilo "% & #x2a;ls'.%. & #x2a ls % & #x2a; ls (pro uživatele se nepovedlo přihlašovací jméno"% & #x2a; ls". Změna hesla se nezdařila. Změna hesla během přihlašování nepodporuje v této verzi systému SQL Server.)|
|18470|14|Přihlášení uživatele se nezdařilo "% & #x2a; ls". Příčina: Účet je disabled.% & #x2a; ls|
|40014|16|Více databázím nelze použít v rámci jedné transakce.|
|40054|16|Tabulky bez seskupený index nejsou podporované v této verzi systému SQL Server. Vytvoření indexu založeného na skupinový a zkuste to znovu.|
|40133|15|Tato operace není podporována v této verzi systému SQL Server.|
|40506|16|ID zadané zabezpečení je neplatný pro tuto verzi systému SQL Server.|
|40507|16|"%. & #x2a; ls' nelze vyvolat s parametry v této verzi systému SQL Server.|
|40508|16|POUŽITÍ příkazu nepodporuje můžete přepínat mezi databází. Pomocí nové připojení se připojit k jiné databázi.|
|40510|16|Příkaz "% & #x2a; ls" není podporováno v této verzi systému SQL Server|
|40511|16|Integrované funkce "% & #x2a; ls" není podporováno v této verzi systému SQL Server.|
|40512|16|Nepoužívají se funkce %ls není podporována v této verzi systému SQL Server.|
|40513|16|Server proměnné "% & #x2a; ls" není podporováno v této verzi systému SQL Server.|
|40514|16|%ls není podporována v této verzi systému SQL Server.|
|40515|16|Odkaz na název databáze a/nebo serveru v "% & #x2a; ls" není podporováno v této verzi systému SQL Server.|
|40516|16|Globální temp objekty nejsou podporované v této verzi systému SQL Server.|
|40517|16|Klíčové slovo nebo fakturu možnost "% & #x2a; ls" není podporováno v této verzi systému SQL Server.|
|40518|16|Příkaz DBCC "% & #x2a; ls" není podporováno v této verzi systému SQL Server.|
|40520|16|S_MSG zabezpečitelné předmětu "%" není podporováno v této verzi serveru SQL Server.|
|40521|16|S_MSG zabezpečitelné předmětu "%" není podporováno v oboru serveru v této verzi systému SQL Server.|
|40522|16|Hlavní "% & #x2a; ls" typ databázi není podporován v této verzi systému SQL Server.|
|40523|16|Implicitní uživatele "% & #x2a; ls" vytváření není podporováno v této verzi systému SQL Server. Vytvoření explicitně uživatele než ho začnete používat.|
|40524|16|Datový typ "% & #x2a; ls" není podporováno v této verzi systému SQL Server.|
|40525|16|S "%.ls" není podporována v této verzi systému SQL Server.|
|40526|16|"%. & #x2a; ls' poskytovatele řádků není podporováno v této verzi systému SQL Server.|
|40527|16|Propojené servery nejsou podporované v této verzi systému SQL Server.|
|40528|16|Uživatelé nelze mapovat certifikáty, asymetrické klíče nebo přihlášení systému Windows v této verzi systému SQL Server.|
|40529|16|Integrované funkce "% & #x2a; ls" v zosobnění kontextu nepodporuje v této verzi systému SQL Server.|
|40532|11|Nelze otevřít server "% & #x2a; ls" požadoval přihlášení. Přihlášení se nezdařila.|
|40553|16|Z důvodu spotřebu paměti nadbytečné byl ukončen relace. Pokuste se změnit dotaz zpracuje menší počet řádků.<br/><br/>*Poznámka:* Omezení počtu `ORDER BY` a `GROUP BY` operace v jazyce Transact-SQL kódu snížení prostřednictvím funkce elektronického požadavky na paměť dotazu.|
|40604|16|Může není vytvořit/ALTER databáze, protože byste překročení kvóty serveru.|
|40606|16|Připojení databází není podporována v této verzi systému SQL Server.|
|40607|16|Přihlášení systému Windows nejsou podporované v této verzi systému SQL Server.|
|40611|16|Servery může obsahovat maximálně 128 pravidel definované.|
|40614|16|Počáteční IP adresa pravidla brány firewall nesmí překročit Koncová IP adres.|
|40615|16|Nelze otevřít serveru: {0} požadoval přihlášení. Klient s IP adresa: {1} není povoleno pro přístup k serveru.  Povolení přístupu pomocí portálu SQL databáze nebo spuštění sp_set_firewall_rule předlohy databázi na vytvořit pravidlo brány firewall pro tuto IP adresu nebo adresu oblast.  Může trvat až pět minut tato změna projevila.|
|40617|16|Název pravidla bránu firewall, který začíná <rule name> je příliš dlouhá. Maximální délka je 128.|
|40618|16|Název pravidla brány firewall nemůže být prázdné.|
|40620|16|Pro uživatele se nepovedlo přihlašovací jméno "% & #x2a; ls". Změna hesla se nezdařila. Změna hesla během přihlašování není podporována v této verzi systému SQL Server.|
|40627|20|Na serveru: {0} a databáze: {1} probíhá. Počkejte několik minut, než znovu.|
|40630|16|Heslo ověření se nezdařilo. Heslo nesplňuje požadavky zásad, protože je příliš krátký.|
|40631|16|Heslo, které jste zadali je příliš dlouhá. Heslo by měla mít víc než 128 znaků.|
|40632|16|Heslo ověření se nezdařilo. Heslo nesplňuje požadavky zásad, protože není dost složité.|
|40636|16|Nelze použít název rezervovaná databáze "% & #x2a; ls" v této operaci.|
|40638|16|Id předplatného neplatné < id předplatného >. Předplatné neexistuje.|
|40639|16|Žádost o neodpovídá schématu: <schema error>.|
|40640|20|Došlo k neočekávané výjimce.|
|40641|16|Do zadaného umístění není platný.|
|40642|17|Server je aktuálně přeplněné. Zkuste znovu později.|
|40643|16|Hodnotu zadanou záhlaví x-ms verze je neplatný.|
|40644|14|Povolit přístup k zadané předplatného se nepovedlo.|
|40645|16|Webu název serveru <servername> nesmí být prázdné nebo obsahovat hodnotu null. Ji můžete jenom být tvořen malá písmena "a"-"z", číslice 0 – 9 a spojovníku. Pomlčka nemusí vést nebo sledu v názvu.|
|40646|16|ID předplatného nesmí být prázdné.|
|40647|16|Předplatné < id předplatného, které neobsahují server název.|
|40648|17|Byly provedeny příliš mnoho požadavků. Opakujte později.|
|40649|16|Není zadán neplatný typ obsahu. Je podporována pouze aplikace/xml.|
|40650|16|Předplatné < id předplatného > neexistuje nebo není připraveno pro operaci.|
|40651|16|Nepodařilo se vytvořit serveru vzhledem k tomu, že předplatné < id předplatného > zakázané.|
|40652|16|Nelze přesunout nebo vytvořit server. Předplatné < id předplatného > bude překročení kvóty serveru.|
|40671|17|Chyba komunikace mezi brány a službu pro správu. Opakujte později.|
|40852|16|Databázi nelze otevřít "%. *ls na serveru "%.* ls' požadoval přihlášení. Přístup k databázi je jenom u povolených pomocí zabezpečení s podporou připojovací řetězec. Přístup k této databázi, upravte připojení řetězce má být zabezpečený na serveru plně kvalifikovaný název domény - server název.database.windows .net by měl upravit tak, aby server název .database. `secure`. windows.net.|
|45168|16|Systém SQL Azure je zatížení a umísťuje horní mez na souběžné operace DB CRUD jednoho serveru (například vytvořit databázi). Server určený v chybové zprávě byla překročena maximální počet souběžné připojení. Zkuste to později.|
|45169|16|Systém SQL azure je zatížení a umísťuje horní mez na počet souběžné serveru operace CRUD pro jeden předplatné (například vytvořit server). Předplatné podle chybová zpráva byla překročena maximální počet souběžné připojení a žádost byla odmítnuta. Zkuste to později.|

## <a name="related-links"></a>Související odkazy

- [Omezení obecné databáze Azure SQL a pokyny](sql-database-general-limitations.md)
- [Omezení zdrojů Azure SQL databáze](sql-database-resource-limits.md)
