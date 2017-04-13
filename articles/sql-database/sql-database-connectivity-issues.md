<properties
    pageTitle="Oprava chyby připojení SQL, přechodná chyba | Microsoft Azure"
    description="Informace o řešení potíží s diagnostikovat a zabránit SQL připojení chybu nebo přechodná chybu v databázi SQL Azure. "
    keywords="připojení k SQL připojovací řetězec, problémy s připojením, přechodná chyby, Chyba připojení"
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="daleche"/>


# <a name="troubleshoot-diagnose-and-prevent-sql-connection-errors-and-transient-errors-for-sql-database"></a>Poradce při potížích s Diagnostika a zabránit chybám připojení SQL a přechodná databáze SQL

Tento článek popisuje, jak zabránit Poradce při potížích s, Diagnostika a zmírnění připojení chybám a přechodná, které klientské aplikace dojde při interakci s databáze SQL Azure. Zjistěte, jak konfigurovat Logika opakování, vytvoření připojovacího řetězce a další nastavení připojení.

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a>Přechodné chyby (chyby přechodná)

Přechodné chyba - také přechodná poruch - má příčinou vyřeší brzy samotné. Občas příčinou přechodná chyb je při systému Azure rychle prostředcích lepší vyrovnávání zatížení různých úloh. Většinu těchto konfigurace událostí často provést 60 sekund. Během tohoto časového rozsahu konfigurace může mít problémy s připojením k databázi SQL Azure. Připojení k databázi SQL Azure aplikace by měl vytvořené očekávat tyto přechodná chyby, provede implementací Logika opakování v kódu místo povrchové uživatelům jako chybné aplikace.

Pokud klientským programem používá ADO.NET, aplikace informaci o chybě přechodná tak, že hodit **SqlException**. Vlastnost **číslo** můžete porovnání proti seznam přechodná chyb v horní části tématu: [kódy chyb SQL pro databáze SQL klientské aplikace](sql-database-develop-error-messages.md).

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Připojení a příkaz

Budete opakovat SQL připojení nebo vytvořit znovu podle těchto věcí:

* **Přechodné chybě dojde při pokusu o připojení**: připojení by měl opakované za zpoždění několik sekund.

* **Dojde k chybě přechodná během dotazu příkaz SQL**: příkaz neměli opakované okamžitě. Místo toho zpožděním připojení by měl být čerstvě vytvořit. Opakovat lze klikněte na příkaz.


<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

### <a name="retry-logic-for-transient-errors"></a>Opakovat logiky pro přechodná chyby


Při obsahují Logika opakování jsou robustnější klientských programů, které občas dojít přechodná chybě.


Při aplikaci komunikaci se databáze SQL Azure až 3 middleware stran, dotaz s dodavatelem zda middleware obsahuje opakovat logiky pro přechodná chyby.

<a id="principles-for-retry" name="principles-for-retry"></a>

#### <a name="principles-for-retry"></a>Zásady pro opakování


- Pokus o otevření připojení má opakovat, pokud je chyba přechodná.


- Příkaz SELECT jazyka SQL, který se nezdaří s chybu přechodná neměli opakované přímo.
 - Místo toho vytvořit nové připojení a zkuste vybrat.


- Selhání se přechodná chyba UPDATE jazyka SQL nové připojení stanovit, než je možné aktualizovat.
 - Logika opakování musíte se ujistit, že transakce celou databázi dokončit, nebo který celé transakce je vrátit zpět.


#### <a name="other-considerations-for-retry"></a>Další informace pro opakování


- Dávkový program, který se automaticky spustí po pracovní doby a která bude dokončena před označením dopoledne, můžou dovolit velmi pacienta dlouho časových intervalů mezi jeho pokusů.


- Uživatelské rozhraní programu by měl účtu pro lidské tendence dát po příliš dlouhá aktivitu počkat.
 - Však řešení nesmí být opakovat každých několik sekund, protože tuto zásadu můžete zahlcení systémové požadavky.


#### <a name="interval-increase-between-retries"></a>Tlačítka zvětšit interval mezi opakování



Doporučujeme zpoždění 5 sekund před prvním opakovat. Opakování zpožděním kratší než 5 sekund rizika neunavily cloudovou službu. Pro každý další pokusy zpoždění růst geometrickou řadou až do velikosti 60 sekund.

Informace o *blokování období* pro klienty, kteří používají ADO.NET je k dispozici v [SQL Server připojení fond (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

Můžete také nastavit maximální počet opakování před vlastním ukončí program.


#### <a name="code-samples-with-retry-logic"></a>Ukázky s Logika opakování


Ukázky s Logika opakování různými jazyky, jsou k dispozici:

- [Knihovny připojení databáze SQL a SQL Server](sql-database-libraries.md)


<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

#### <a name="test-your-retry-logic"></a>Testování logiky opakování


Testování logiky opakovat, musíte simulovat nebo dojít k chybě, než lze opravit během spuštění aplikace.


##### <a name="test-by-disconnecting-from-the-network"></a>Otestujte odpojení ze sítě


Jedním ze způsobů můžete otestovat logiky opakovat je odpojit klientského počítače se ze sítě je spuštěná program. Chyba bude:

- **SqlException.Number** = 11001
- Zpráva: "takový hostitel známa"


Jako součást prvním pokusu Opakovat můžete aplikaci opravte pravopisnou chybu a pokuste se připojit.


Nastavte u této praktické, odpojte počítač ze sítě než spustíte aplikaci. Potom aplikaci rozpozná běhu parametr, který způsobuje program:

1. Přidejte do svého seznamu chybám považovat za přechodové dočasně 11001.
2. Odeslání jeho první připojení jako obvykle.
3. Po zachyceny chybu odeberte 11001 ze seznamu.
4. Zobrazí se zpráva uživatele k připojení počítače k síti.
 - Zastavte ukazatel myši dalšího spuštění pomocí metody **Console.ReadLine** nebo dialogové okno tlačítko OK. Stisknutí klávesy Enter po počítač připojen k síti.
5. Odeslání znovu připojit, očekává úspěch.


##### <a name="test-by-misspelling-the-database-name-when-connecting"></a>Otestujte Chyba v při připojování pravopisu název databáze


Aplikaci můžete záměrně chybně uživatelské jméno před prvním pokusu o připojení. Chyba bude:

- **SqlException.Number** = 18456
- Zpráva: "přihlášení se nezdařilo pro uživatele"WRONG_MyUserName"."


Jako součást prvním pokusu Opakovat můžete aplikaci opravte pravopisnou chybu a pokuste se připojit.


Nastavte u této praktické, může aplikaci rozpoznat běhu parametr, který způsobuje program:

1. Přidejte do svého seznamu chybám považovat za přechodové dočasně 18456.
2. "WRONG_" záměrně dodejte uživatelské jméno.
3. Po zachyceny chybu odeberte 18456 ze seznamu.
4. Odebrání uživatelské jméno "WRONG_".
5. Odeslání znovu připojit, očekává úspěch.

<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

### <a name="net-sqlconnection-parameters-for-connection-retry"></a>Parametry .NET SqlConnection pro připojení opakování


Pokud klientským programem ke kterému je připojen k databázi SQL Azure pomocí .NET Framework třídy **System.Data.SqlClient.SqlConnection**, měli byste použít .NET 4.6.1 nebo novější, můžete využít jeho funkce opakovat připojení. Podrobnosti o funkci [tady](http://go.microsoft.com/fwlink/?linkid=393996).


<!--
2015-11-30, FwLink 393996 points to dn632678.aspx, which links to a downloadable .docx related to SqlClient and SQL Server 2014.
-->


Při vytváření [připojovací řetězec](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) pro objekt **SqlConnection** by měl koordinaci hodnot mezi následujících parametrů:

- ConnectRetryCount &nbsp; &nbsp; *(výchozí hodnota je 1. Rozsah je od 0 do 255.)*
- ConnectRetryInterval &nbsp; &nbsp; *(výchozí hodnota je 1 druhé. Oblast představuje 1 až 60.)*
- Časový limit připojení &nbsp; &nbsp; *(výchozí hodnota je 15 sekund. Rozsah je 0 až 2147483647)*


Konkrétně zvolené hodnoty připomínat následující rovnosti true:

- Časový limit připojení = ConnectRetryCount * ConnectionRetryInterval

Například pokud počet = 3 a interval = 10 sekund, časový limit pouze 29 sekund by úplně dát systému dostatek času pro jeho 3 a konečné opakovat v připojení: 29 < 3 * 10.

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Připojení a příkaz


Parametry **ConnectRetryCount** a **ConnectRetryInterval** nechat **SqlConnection** objektu opakovat operace připojení bez informací nebo bothering aplikaci, třeba návratu ovládacích do aplikace. Pokusy může dojít v následujících situacích:

- volání metody mySqlConnection.Open
- volání metody mySqlConnection.Execute

Není subtlety. Pokud přechodná chybě dojde při spuštění *dotazu* , objektu **SqlConnection** opakované pokusy o operace připojení a je určitě opakované pokusy o dotazu. Však **SqlConnection** velmi rychle kontroluje připojení před odesláním dotazu pro spuštění. Pokud Rychlá kontrola zjistí, že připojení, **SqlConnection** opakování operace připojení. Pokud opakovat úspěšné, dotazem jsou odeslány pro spuštění


#### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a>ConnectRetryCount zkombinovat s Logika opakování aplikace?

Předpokládejme, že má aplikace logika robustní vlastní opakování. Mohou opakovat operace připojení 4 časy. Pokud přidáte **ConnectRetryInterval** a **ConnectRetryCount** = 3 připojovací řetězec zvětšíte počet opakování 4 * 3 = 12 opakování. Nebudete mít v úmyslu vysoké počet opakování.

<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-to-azure-sql-database"></a>Připojení k databázi Azure SQL

<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a>Připojení: Připojovací řetězec


Potřebné pro připojení k databázi SQL Azure připojovací řetězec je mírně liší od řetězec pro připojení k systému Microsoft SQL Server. Z [Portálu Azure](https://portal.azure.com/)můžete zkopírovat připojovací řetězec pro databázi.


[AZURE.INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]


<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a>Připojení: IP adresa


Musíte nakonfigurovat databáze SQL serveru přijmout sdělení IP adresu počítače, který je hostitelem klientským programem. To uděláte tak, že nastavení brány firewall přes [Azure portálu](https://portal.azure.com/)pro úpravy.


Pokud je zapomenete konfigurace IP adresu, aplikace se nepovede se vám hodit chybová zpráva oznamující potřebné IP adresu.


[AZURE.INCLUDE [sql-database-include-ip-address-22-v12portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]


Další informace najdete v tématu: [jak: Nakonfigurujte nastavení brány firewall na databáze SQL](sql-database-configure-firewall-settings.md)


<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a>Připojení: porty


Obvykle stačí zajistěte, aby byl port 1433 otevřené pro odchozí komunikaci v počítači, který je hostitelem klientským programem.


Třeba když klientským programem je hostovaný ve počítače s Windows, bránu Windows Firewall podle hostiteli umožňuje otevřít port 1433:


1. Otevřete ovládací panely
2. &gt;Všechny položky Ovládacích panelů
3. &gt;Brána Firewall systému Windows
4. &gt;Upřesňující nastavení
5. &gt;Odchozího pravidla
6. &gt;Akce
7. &gt;Nové pravidlo


Pokud klientským programem je hostovaný ve Azure virtuálního počítače (OM), přečtěte si:<br/>[Porty za 1433 ADO.NET 4.5 a V12 databáze SQL](sql-database-develop-direct-route-ports-adonet-v12.md).


Základní informace o konfigurace portů a IP adresu, najdete v tématu: [Brána firewall databáze SQL Azure](sql-database-firewall-configure.md)


<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-461"></a>Připojení: ADO.NET 4.6.1


Pokud váš program používá třídy ADO.NET jako **System.Data.SqlClient.SqlConnection** pro připojení k databázi SQL Azure, doporučujeme použít .NET Framework verze 4.6.1 nebo vyšší.


ADO.NET 4.6.1:

- Databáze SQL Azure existuje Zvýšená spolehlivost: při otevření připojení pomocí metody **SqlConnection.Open** . Metody **Open** teď zahrnuje nejlepší mechanismy opakovat plánování řízené úsilí odpověď přechodná chyby, u některých chyb v rámci doby vypršení časového limitu připojení.
- Podporuje fond připojení. Platí to i pro efektivní ověření, že funguje objekt připojení nabízí aplikace.



Pokud používáte připojení objektu z fond připojení, doporučujeme, aby aplikace dočasně ukončit připojení, když není okamžitě s ním pracovat. Opětovné otevření připojení není drahé je možné vytvářet nové připojení.


Pokud používáte ADO.NET 4.0 nebo starší, doporučujeme upgradovat na nejnovější ADO.NET.

- K listopad 2015 můžete [Stáhnout ADO.NET 4.6.1](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).


<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a>Diagnostika

<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a>Diagnostika: Testování možnosti nástroje můžete připojit.


Pokud váš program selhává připojení k databázi SQL Azure, jednu možnost diagnostické je zkoušíte připojit přes sady nástrojů. Nástroj by v ideálním případě připojit pomocí stejného knihovnu, která používá aplikaci.


Na libovolném počítači s Windows můžete vyzkoušet tyto nástroje:

- SQL Server Management Studio (ssms.exe), který se připojuje pomocí ADO.NET.
- Sqlcmd.exe připojí pomocí [rozhraní ODBC](http://msdn.microsoft.com/library/jj730308.aspx).


Po připojení ověřte, zda funguje krátké SQL výběrový dotaz.


<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-the-open-ports"></a>Diagnostika: Zkontrolujte otevřít porty


Předpokládejme, že máte podezření, že pokusy o připojení se nedaří kvůli problémům s portem k. Na vašem počítači poběží nástroj, který zprávy o konfiguraci portů.


Následující nástroje na Linux může být užitečné:

- `netstat -nap`
- `nmap -sS -O 127.0.0.1`
 - (Hodnotu příklad být svoji IP adresu změnit).


Ve Windows [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) nástroj může být užitečné. Tady je příklad spuštění, dotazovaném situace port serveru databáze SQL Azure, a které byl spuštěn na přenosném počítači:


```
[C:\Users\johndoe\]
>> portqry.exe -n johndoesvr9.database.windows.net -p tcp -e 1433

Querying target system called:
 johndoesvr9.database.windows.net

Attempting to resolve name to IP address...
Name resolved to 23.100.117.95

querying...
TCP port 1433 (ms-sql-s service): LISTENING

[C:\Users\johndoe\]
>>
```


<a id="g-diagnostics-log-your-errors" name="g-diagnostics-log-your-errors"></a>

### <a name="diagnostics-log-your-errors"></a>Diagnostiky: Protokolování chyby


K chybovému problému je někdy nejlepší zjistit tak, že zjišťování obecné vzorek přes dnech nebo týdnech.


Klient může pomoci při Diagnostika funkcí protokolování všechny chyby, kterou najde. Je možné sladit položky protokolu s údaji o chybě interně databáze SQL Azure samotné protokolování.


Organizace knihovny 6 (EntLib60) nabízí třídy .NET spravovaných kvůli usnadnění protokolování:

- [5 – stejně snadná jako spadající mimo protokol: pomocí aplikace blok protokolování](http://msdn.microsoft.com/library/dn440731.aspx)


<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a>Diagnostika: Prohlédněte systém protokoly chyb


Tady jsou některé příkazy vyberte jazyce Transact-SQL tohoto dotazu protokolů chyb a dalších informací.


| Dotaz protokolu | Popis |
| :-- | :-- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` | Zobrazení [sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) poskytuje informace o jednotlivých událostí, včetně některých, které mohou způsobit přechodná chyby nebo selhání připojení.<br/><br/>**Start_time** nebo **end_time** hodnot v ideálním případě můžete porovnat s informacemi o kdy došlo k potížím klientským programem.<br/><br/>**TIP:** Musí se připojíte ke **hlavní** databáze, kterou chcete spustit. |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` | Zobrazení [sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) nabízí agregované počty typů událostí pro další diagnostiky.<br/><br/>**TIP:** Musí se připojíte ke **hlavní** databáze, kterou chcete spustit. |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-the-sql-database-log"></a>Diagnostika: Hledání problém událostí v protokolu SQL databáze


Můžete vyhledávat záznamy o událostech problém v protokolu databáze SQL Azure. Vyzkoušejte následující jazyce Transact-SQL výběrový dotaz v **hlavní** databáze:


```
SELECT
   object_name
  ,CAST(f.event_data as XML).value
      ('(/event/@timestamp)[1]', 'datetime2')                      AS [timestamp]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="error"]/value)[1]', 'int')             AS [error]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="state"]/value)[1]', 'int')             AS [state]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="is_success"]/value)[1]', 'bit')        AS [is_success]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="database_name"]/value)[1]', 'sysname') AS [database_name]
FROM
  sys.fn_xe_telemetry_blob_target_read_file('el', null, null, null) AS f
WHERE
  object_name != 'login_event'  -- Login events are numerous.
  and
  '2015-06-21' < CAST(f.event_data as XML).value
        ('(/event/@timestamp)[1]', 'datetime2')
ORDER BY
  [timestamp] DESC
;
```


#### <a name="a-few-returned-rows-from-sysfnxetelemetryblobtargetreadfile"></a>Několik vrácených řádků z sys.fn_xe_telemetry_blob_target_read_file


Je další řádek vrácené může vypadat takto. Zobrazené hodnoty null jsou často není null v dalších řádcích.


```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```


<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a>Knihovna Enterprise 6


Pole organizace knihovny 6 (EntLib60) je rámec .NET tříd, který umožňuje provádět robustní klientů cloudové služby, jedna z nich je služba databáze SQL Azure. Můžete vyhledat témata snaží každou oblast, ve kterém mohou pomoci EntLib60 první webu:

- [Knihovna Enterprise 6 – duben 2013](http://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx)


Logika opakování zpracování přechodná chyb je jedné oblasti, ve kterém mohou pomoci EntLib60:

- [4 – perseverance, tajná všechny vítězství: pomocí aplikace blok přechodná poruch zpracování](http://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx)


> [AZURE.NOTE] Zdrojový kód EntLib60 je dostupný u veřejných [Stáhnout](http://go.microsoft.com/fwlink/p/?LinkID=290898). Společnost Microsoft nemá žádné plány provádět další funkce aktualizace nebo aktualizace údržbu EntLib.

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a>Třídy EntLib60 pro přechodná chyby a opakování


Následující EntLib60 třídy jsou užitečné zejména pro Logika opakování. Vše Toto jsou v nebo další v části obor názvů **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:

*V názvů* *Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling* *:*

- **RetryPolicy** třídy
 - Metoda **ExecuteAction**


- **ExponentialBackoff** třídy


- **SqlDatabaseTransientErrorDetectionStrategy** třídy


- **ReliableSqlConnection** třídy
 - Metody **ExecuteCommand**


V názvů **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:

- **AlwaysTransientErrorDetectionStrategy** třídy

- **NeverTransientErrorDetectionStrategy** třídy


Tady jsou odkazy na informace o EntLib60:

- Bezplatné [rezervovat soubor ke stažení: Příručka pro vývojáře do knihovny Microsoft Enterprise, 2 Edition](http://www.microsoft.com/download/details.aspx?id=41145)

- Doporučené postupy: [opakování obecné pokyny](../best-practices-retry-general.md) má pracovníků podrobný popis logiky opakovat.

- Stažení NuGet knihovny [Enterprise – přechodná zpracování poruch aplikace blok 6.0](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-the-logging-block"></a>EntLib60: Protokolování blokování


- Blokování protokolování je velmi flexibilní a konfigurovat řešení, které umožňuje:
 - Vytváření a ukládání zpráv protokolu v širokou škálu umístění.
 - Zařadit do kategorií a filtrování zpráv.
 - Shromáždit kontextové informace, které jsou užitečné pro ladění a trasování i pro auditování a obecné protokolování požadavky.


- Blokování protokolování abstrahuje funkce protokolování cíl protokolu tak, že kód aplikace konzistentní, bez ohledu umístění a typ protokolování cílového úložiště.


Podrobnosti najdete v článku: [5 – jako snadno jako spadající mimo protokolu: pomocí aplikace blok protokolování](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a>EntLib60 IsTransient metoda zdrojového kódu


Pak ze třídy **SqlDatabaseTransientErrorDetectionStrategy** je C# zdrojový kód metodu **IsTransient** . Zdrojový kód vysvětluje, které chyby byly považovány za přechodná a worthy opakování od duben 2013.

Spoustu řádky **//comment** mohly být odebrány z této kopie zvýraznit čitelnosti.


```
public bool IsTransient(Exception ex)
{
  if (ex != null)
  {
    SqlException sqlException;
    if ((sqlException = ex as SqlException) != null)
    {
      // Enumerate through all errors found in the exception.
      foreach (SqlError err in sqlException.Errors)
      {
        switch (err.Number)
        {
            // SQL Error Code: 40501
            // The service is currently busy. Retry the request after 10 seconds.
            // Code: (reason code to be decoded).
          case ThrottlingCondition.ThrottlingErrorNumber:
            // Decode the reason code from the error message to
            // determine the grounds for throttling.
            var condition = ThrottlingCondition.FromError(err);

            // Attach the decoded values as additional attributes to
            // the original SQL exception.
            sqlException.Data[condition.ThrottlingMode.GetType().Name] =
              condition.ThrottlingMode.ToString();
            sqlException.Data[condition.GetType().Name] = condition;

            return true;

          case 10928:
          case 10929:
          case 10053:
          case 10054:
          case 10060:
          case 40197:
          case 40540:
          case 40613:
          case 40143:
          case 233:
          case 64:
            // DBNETLIB Error Code: 20
            // The instance of SQL Server you attempted to connect to
            // does not support encryption.
          case (int)ProcessNetLibErrorCode.EncryptionNotSupported:
            return true;
        }
      }
    }
    else if (ex is TimeoutException)
    {
      return true;
    }
    else
    {
      EntityException entityException;
      if ((entityException = ex as EntityException) != null)
      {
        return this.IsTransient(entityException.InnerException);
      }
    }
  }

  return false;
}
```


## <a name="next-steps"></a>Další kroky

- Návody na řešení jiných běžné problémy s připojením databáze SQL Azure, navštěvujte blog o [Poradce při potížích připojení k databázi SQL Azure](sql-database-troubleshoot-common-connection-issues.md).

- [Připojení k serveru SQL fond (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx)


- [*Opakování* je že Apache 2.0 licencovaný univerzální opakování knihovny napsané v **Python**zjednodušit úkolu obrazovky pro přidání opakovat chování prakticky cokoliv.](https://pypi.python.org/pypi/retrying)
