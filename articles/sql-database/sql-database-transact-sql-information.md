<properties
   pageTitle="Nepodporované v databázi Azure SQL T-SQL | Microsoft Azure"
   description="Skalární funkce příkazy, které jsou menší než plně podporované v databázi SQL Azure"
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="08/30/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="azure-sql-database-transact-sql-differences"></a>Rozdíly SQL databáze Transact-SQL Azure


Většina funkcí jazyce Transact-SQL, které aplikace závisí na podporují služby Microsoft SQL Server a databázi SQL Azure. Seznam podporovaných funkcí pro aplikace takto:

- Datové typy.
- Operátory.
- Funkce String, aritmetický logické a kurzor.

Však databáze SQL Azure slouží k oddělení není funkce z libovolné závislost na **hlavní** databáze. V důsledku mnoho aktivit úrovni serveru je vhodný pro databáze SQL a nejsou podporované. Funkce, které jsou změněny na serveru SQL Server nepodporuje obecně databáze SQL.

> [AZURE.NOTE]
> Toto téma popisuje funkce, které jsou dostupné pro databáze SQL při aktualizaci na aktuální verzi; V12 databáze SQL. Další informace o V12 najdete v tématu [SQL databáze V12 co je nového](sql-database-v12-whats-new.md).

V následujících částech Seznam funkcí, které jsou částečně podporovaná funkce a funkce, které nejsou plně podporované.


## <a name="features-partially-supported-in-sql-database-v12"></a>Funkce částečně podporované v SQL databázi V12

SQL databáze V12 podporuje některých, ale ne všechny argumenty, které jsou v odpovídající příkazy SQL serveru 2016 Transact-SQL. Například příkaz CREATE PROCEDURE neexistuje však všechny možnosti CREATE PROCEDURE nejsou k dispozici. Naleznete v tématech propojené syntaxe podrobnosti o podporovaných oblastí jednotlivých údajů.

- Databáze: [Vytvoření](https://msdn.microsoft.com/library/dn268335.aspx )/[změnit](https://msdn.microsoft.com/library/ms174269.aspx)
- DMVs obecně pro jsou dostupné funkce, které jsou k dispozici.
- Funkce: [Vytvoření](https://msdn.microsoft.com/library/ms186755.aspx)/[změnit (funkce)](https://msdn.microsoft.com/library/ms186967.aspx)
- [UKONČENÍ](https://msdn.microsoft.com/library/ms173730.aspx) 
- Přihlášení: [Vytvoření](https://msdn.microsoft.com/library/ms189751.aspx)/[změnit přihlášení](https://msdn.microsoft.com/library/ms189828.aspx)
- Uložené procedury: [Vytvoření](https://msdn.microsoft.com/library/ms187926.aspx)/[ALTER PROCEDURE](https://msdn.microsoft.com/library/ms189762.aspx)
- Tabulky: [Vytvoření](https://msdn.microsoft.com/library/dn305849.aspx)/[změnit](https://msdn.microsoft.com/library/ms190273.aspx)
- Typy (vlastní): [Vytvoření typ](https://msdn.microsoft.com/library/ms175007.aspx)
- Uživatelé: [Vytvoření](https://msdn.microsoft.com/library/ms173463.aspx)/[Změnit uživatele](https://msdn.microsoft.com/library/ms176060.aspx)
- Zobrazení: [Vytvoření](https://msdn.microsoft.com/library/ms187956.aspx)/[měnit zobrazení](https://msdn.microsoft.com/library/ms173846.aspx)

## <a name="features-not-supported-in-sql-database"></a>Funkce nejsou podporované v databázi SQL

- Řazení systémových objektů
- Připojení související: koncový bod výkazech, ORIGINAL_DB_NAME. Databáze SQL nepodporuje ověřování systému Windows, ale podporuje podobné ověřování Azure Active Directory. Některé typy ověřování vyžadují nejnovější verzi SSMS. Další informace najdete v tématu [připojení k databázi SQL nebo SQL datový sklad tak, že pomocí Azure Active Directory, ověřování](sql-database-aad-authentication.md).
- Křížové dotazy na databázi část názvů tři nebo čtyři. (Jen pro čtení databáze Křížové dotazy jsou podporovány pomocí [pružná databázových dotazů](sql-database-elastic-query-overview.md).)
- Zřetězení vlastnictví křížového databáze, DŮVĚRYHODNÝ nastavení
- Shromažďování dat
- Diagramy databáze
- Hromadné databáze
- DATABASEPROPERTY (použijte DATABASEPROPERTYEX)
- Přihlášení jako EXECUTE
- Šifrování: extensible správy klíčů
- Událostí: události, oznámení událostí, dotaz oznámení
- Funkce související s umístění souboru databáze, velikost a databázové soubory, které jsou automaticky spravuje Microsoft Azure.
- Funkce, které se týkají dostupnost, které spravují pomocí účtu Microsoft Azure: zálohování, obnovení AlwaysOn odrážející strukturu databáze, protokol, režimech obnovení. Další informace, najdete v článku Azure SQL databáze zálohování a obnovení.
- Funkce, které potřebují protokolu čtenáře provozu v databázi SQL: nabízená replikace, shromažďování dat změn.
- Funkce, které potřebují Agent SQL serveru nebo databáze MSDB: úlohy, upozornění a operátorů, založené na zásadách správy, mail databáze, servery centrální správy.
- FILESTREAM
- Funkce: fn_get_sql fn_virtualfilestats, fn_virtualservernodes
- Globální dočasné tabulky
- Nastavení serveru související s hardwarem: paměti pracovních podprocesů, spřažení procesoru, sledování příznaky, atd. Použijte úrovně služeb
- HAS_DBACCESS
- ODSTRANĚNÍ STATISTIKA PROJEKTU
- Propojené servery OTEVŘÍTDOTAZ, OPENROWSET, OPENDATASOURCE, HROMADNÉ vložení a čtyři názvy
- Hlavní/cílových serverů
- .NET framework [CLR integrace se serverem SQL Server](http://msdn.microsoft.com/library/ms254963.aspx)
- Správce prostředků k dosažení
- Sémantického hledání
- Server pověření. Použít databázi místo toho omezené přihlašovací údaje.
- Server úroveň položky: sys.login_token role, IS_SRVROLEMEMBER, serveru. Oprávnění na úrovni serveru nejsou k dispozici, když některé nahrazují databáze úrovně oprávnění. Některé úrovni serveru DMVs nejsou k dispozici, když některé nahrazeny DMVs úrovni databáze.
- Bez serveru express: localdb instance uživatele
- Zprostředkovatel služeb
- NASTAVENÍ REMOTE_PROC_TRANSACTIONS
- VYPNUTÍ
- sp_addmessage
- Možnosti sp_configure a změňte konfiguraci. Některé možnosti jsou k dispozici pomocí [Změnit rozsah konfigurace databáze](https://msdn.microsoft.com/library/mt629158.aspx).
- sp_helpuser
- sp_migrate_user_to_contained
- SQL Server auditování. Databáze SQL auditování místo toho použijte.
- SQL Server nástroje
- Sledování SQL serveru
- Trasovat příznaků. Některé položky příznaku sledování přesunou do režimu kompatibility.
- Ladění Transact-SQL
- Aktivace: Omezené serveru nebo přihlášení aktivačních událostí
- POUŽITÍ příkazu: Chcete-li změnit kontextu databáze na jinou databázi, musí vytvořit nové připojení do nové databáze.


## <a name="full-transact-sql-reference"></a>Úplný odkaz jazyce Transact-SQL

Další informace o jazyce Transact-SQL gramatiky používání a příklady naleznete v [Jazyce Transact-SQL odkaz (databázový stroj)](https://msdn.microsoft.com/library/bb510741.aspx) v SQL Server knihy Online. 

### <a name="about-the-applies-to-tags"></a>Značky "Platí pro"

Odkaz na jazyce Transact-SQL obsahuje témat týkajících se verze SQL serveru 2008 pro prezentace. Pod obsahuje téma název je ikona pruhové, uvádějící čtyři platformy SQL serveru a označující použitelnost. Příklad skupiny dostupnosti byly vydané ve verzi SQL Server 2012. V tématu [Vytvoření skupiny AVAILABILTY](https://msdn.microsoft.com/library/ff878399.aspx) označuje, že příkazu použije k ** SQL Server (počínaje číslem 2012). Příkaz, nebudou mít vliv na SQL Server 2008, SQL Server 2008 R2, databáze SQL Azure, Azure SQL Data Warehouse nebo paralelní datový sklad.

V některých případech obecné Předmět téma lze použít v produktu, ale jsou menší rozdíly mezi produkty. Rozdíly jsou uvedeny v středních bodů v tématu podle potřeby.

