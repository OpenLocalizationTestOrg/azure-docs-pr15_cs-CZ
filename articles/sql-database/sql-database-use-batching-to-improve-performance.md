 <properties
    pageTitle="Jak zvýšit výkon aplikace databáze SQL Azure pomocí dávky"
    description="Téma obsahuje důkaz této dávkování operací databázi výrazně imroves rychlost a škálovatelnost aplikací databáze SQL Azure. I když tyto dávkování techniky práce pro všechny databáze SQL serveru, na článek se zaměřuje na Azure."
    services="sql-database"
    documentationCenter="na"
    authors="annemill"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="07/12/2016"
    ms.author="annemill" />

# <a name="how-to-use-batching-to-improve-sql-database-application-performance"></a>Jak používat dávky ke zvýšení výkonu aplikace databáze SQL

Dávkové operace k databázi SQL Azure výrazně zlepšuje výkon a škálovatelnost aplikací. K pochopit výhody, první část Tento článek popisuje některé výsledky testů ukázkové porovnává sekvenční a dávkový požadavky k databázi SQL. Zbývající část tohoto článku zobrazuje technik, scénáře a důležité informace týkající se použití dávky úspěšně v aplikacích Azure.

**Autoři**: Jason Roth, Silvano Coriani Trent Swanson (celou škálu 180 Inc.)

**Recenzenti**: Conor Cunningham, Jan Thomassy

## <a name="why-is-batching-important-for-sql-database"></a>Proč je dávky důležité databáze SQL?
Dávkové volání pro vzdálené služby je známý strategie zvýšit výkon a škálovatelnost. Jsou pevné náklady zpracování všech interakcím se vzdálené služby, jako je serializace, přenos sítě a rekonstrukce. Balení kolik transakcí samostatné do jedné dávce minimalizuje tyto náklady.

V tomto dokumentu chcete prozkoumat různých databáze SQL dávky strategie a scénáře. I když tyto strategie jsou důležitá také pro místní aplikace, které používají SQL serveru, existuje několik důvodů ke zvýraznění použití dávky databáze SQL:

- Obsahuje potenciálně větší latence sítě přístup k databázi SQL, zejména pokud pracujete s SQL databáze z mimo stejné Microsoft Azure datacentra.
- Víceklientské vlastnosti databáze SQL znamená, že účinnosti dat aplikace access vrstvy odpovídá celkové škálovatelnost databáze. Databáze SQL musí všichni jednoho klienta a uživatelé zabránit přivlastňuje databáze zdroje proti jiných klientů. Odpověď na použití více než předdefinované kvóty databáze SQL zmenšení výkon nebo odpoví omezení výjimky. Efektivity, například dávky, umožňují víc práce na databáze SQL před dosažením tato omezení. 
- Dávkové je také efektivní pro architektury, které používají více databázím (sharding). Efektivity interakce s každé jednotky databáze je stále klíčový faktor celkové škálovatelnost. 

Jednou z výhod používání databáze SQL je, že nemusíte spravovat servery, které hostují databáze. Tato spravovaných infrastruktura znamená však také, že máte jinak přemýšlet o optimalizaci databáze. Už může samostatně zlepšit hardwaru nebo síťové infrastruktury databáze. Microsoft Azure mezery nastavuje těchto prostředí. Oblast hlavního, který můžete ovládat je interakci aplikace se databáze SQL. Dávkové je jedním z těchto optimalizací. 

První část papíru kontroluje různých dávkování technik pro .NET aplikace, které používají databáze SQL. Poslední dvě části vysvětlovat dávkování pokyny a scénáře.

## <a name="batching-strategies"></a>Dávkové strategie

### <a name="note-about-timing-results-in-this-topic"></a>Poznámka o načasování výsledky v tomto tématu
>[AZURE.NOTE] Výsledky nejsou ukazatele, ale jsou určeny zobrazujícího **relativní výkon**. Časování vycházejí průměrnou délku nejméně 10 zkušební jízdy. Operace se vloží do prázdnou tabulku. Tyto testy byly naměřené V12 pre a nutně neodpovídají výkonu, může dojít v databázi V12 pomocí nového [– úrovně služeb](sql-database-service-tiers.md). Relativní výhodou dávkování postup by měl být stejný.

### <a name="transactions"></a>Transakce
Zdá nestandardní zahájíte seznámit s dávky tak, že projednávat transakce. Ale pomocí klienta transakce obsahuje jemné serverovou dávkování efekt, který zlepšuje výkon. A transakce přidáním s jenom několik řádků kódu, takže poskytují rychlý způsob, jak zlepšit výkon sekvenční operací.

Zvažte následující C# kód, který obsahuje řadu vložení operace a aktualizace na jednoduchou tabulku.

    List<string> dbOperations = new List<string>();
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 1");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 2");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 3");
    dbOperations.Add("insert MyTable values ('new value',1)");
    dbOperations.Add("insert MyTable values ('new value',2)");
    dbOperations.Add("insert MyTable values ('new value',3)");

Následující kód ADO.NET postupně provádí tyto operace.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();
    
        foreach(string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn);
            cmd.ExecuteNonQuery();                   
        }
    }

Nejlepší způsob, jak optimalizovat tento kód je implementovat některý z klienta dávky tyto volání. Ale jednoduchý způsob, jak zvýšit výkon tento kód jednoduše obtékání sekvence hovory v transakci. Tady je stejný kód, který používá transakce.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();
        SqlTransaction transaction = conn.BeginTransaction();
    
        foreach (string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn, transaction);
            cmd.ExecuteNonQuery();
        }
    
        transaction.Commit();
    }

Transakce jsou používány v obou těchto příkladů. V prvním příkladu je každý jednotlivé volání implicitní transakce. Ve druhém příkladu zalamuje explicitní transakce všechny volání. Za v dokumentaci k [zápis dokončováním transakční protokol](https://msdn.microsoft.com/library/ms186259.aspx)jsou záznamů protokolu vyprázdnění na disk, když potvrdí transakce. Takže přidáním dalších hovorů v rámci transakce zápis transakční protokol zpozdit až transakce. Ve skutečnosti povolíte dávky pro zapíše do protokolu transakce na server.

Následující tabulka uvádí některé ad-hoc testování výsledky. Testů provedených stejné sekvenční vloží mezi šablonami a bez transakce. Pro další pohledu první sadě testů spustili vzdáleně z přenosný počítač k databázi Microsoft Azure. Druhá sadu testů spustili z Cloudová služba a databáze oba nacházejí v rámci stejné datacentra Microsoft Azure (západ USA). Následující tabulka zobrazuje dobu trvání v milisekundách sekvenční vloží mezi šablonami a bez transakce.

**Místní na Azure**:

| Operace | Žádná transakce ([ms) | Transakce ([ms) |
|---|---|---|
| 1 | 130 | 402 |
| 10 | 1208 | . 1226 |
| 100 | 12662 | 10395 |
| 1 000 | 128852 | 102917 |


**Azure k Azure (stejné datacentra)**:

| Operace | Žádná transakce ([ms) | Transakce ([ms) |
|---|---|---|
| 1 | 21 | 26 |
| 10 | 220 | 56 |
| 100 | 2145 | 341 |
| 1 000 | 21479 | 2756 |

>[AZURE.NOTE] Výsledky nejsou ukazatele. Zobrazuje [upozornění o načasování výsledky v tomto tématu](#note-about-timing-results-in-this-topic).

Podle předchozí výsledky testů obtékání jedné operace v transakce ve skutečnosti sníží výkonu. Ale jak zvýšit počet operací v rámci jedné transakce zlepšení výkonu změní více označena. Rozdíl výkonu při taky výraznějších všechny operace rozmezích Microsoft Azure datacentra. Lepší latence používání mimo datacentra Microsoft Azure SQL databáze z ruší zvýšení výkonu používání transakce.

Sice využívání transakce můžete dosáhnout zvýšení výkonu, dál [dodržovat doporučené postupy pro transakce a připojení](https://msdn.microsoft.com/library/ms187484.aspx). Zachovat co nejkratší transakce a zavřete připojení k databázi po dokončení práce. Pomocí příkazu v předchozím příkladu zajišťuje, že je zavřený připojení dokončení bloku další kódu.

Předchozí příklad ukazuje, které můžete přidat místní transakce k jakýkoli kód ADO.NET se dvěma řádky. Transakce představují rychlý způsob, jak zlepšit výkon kód, který provádí sekvenční vložení, aktualizovat a odstraňovat operace. Však nejrychlejší výkon, zvažte možnost kód na Využijte výhod dávky klienta, třeba parametry vracející tabulku.

Další informace o transakce v ADO.NET najdete v článku [Místní transakce v ADO.NET](https://msdn.microsoft.com/library/vstudio/2k2hy99x.aspx).

### <a name="table-valued-parameters"></a>Parametry vracející tabulku
Vracející tabulku parametry podporují typy uživatelem definovaných tabulky jako parametrů v jazyce Transact-SQL příkazy, uložené procedury a funkce. Tento postup dávkování klientských umožňuje odeslat více řádků dat v rámci parametr vracející tabulku. Použití parametrů vracející tabulku, nejdřív Definujte typ table. Následující příkaz Transact-SQL vytvoří typ tabulku s názvem **MyTableType**.

    CREATE TYPE MyTableType AS TABLE 
    ( mytext TEXT,
      num INT );
 

V kódu vytvořte **datové tabulky** se přesně stejné názvy a typy typ table. Přidat tento **datové tabulky** parametr v textu dotazu nebo uložené volání procedur. Následující příklad ukazuje tento postup:

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        DataTable table = new DataTable();
        // Add columns and rows. The following is a simple example.
        table.Columns.Add("mytext", typeof(string));
        table.Columns.Add("num", typeof(int));    
        for (var i = 0; i < 10; i++)
        {
            table.Rows.Add(DateTime.Now.ToString(), DateTime.Now.Millisecond);
        }
    
        SqlCommand cmd = new SqlCommand(
            "INSERT INTO MyTable(mytext, num) SELECT mytext, num FROM @TestTvp",
            connection);
                    
        cmd.Parameters.Add(
            new SqlParameter()
            {
                ParameterName = "@TestTvp",
                SqlDbType = SqlDbType.Structured,
                TypeName = "MyTableType",
                Value = table,
            });
    
        cmd.ExecuteNonQuery();
    }

V předchozím příkladu, objekt **SqlCommand** vloží řádky z parametr vracející tabulku **@TestTvp**. Dříve vytvořené objekt **datové tabulky** přiřazený metodu **SqlCommand.Parameters.Add** tento parametr. Dávkové vloží v jednom volání výrazně zvyšuje výkon přes sekvenční vloží.

Abychom vylepšili dále v předchozím příkladu, použití uložené procedury namísto příkazu textový. Následující příkaz Transact-SQL vytvoří uložené procedury, která přijímá parametr **SimpleTestTableType** vracející tabulku.

    CREATE PROCEDURE [dbo].[sp_InsertRows] 
    @TestTvp as MyTableType READONLY
    AS
    BEGIN
    INSERT INTO MyTable(mytext, num) 
    SELECT mytext, num FROM @TestTvp
    END
    GO

Změňte deklaraci **SqlCommand** objekt v předchozím příkladu kód následujícím způsobem.

    SqlCommand cmd = new SqlCommand("sp_InsertRows", connection);
    cmd.CommandType = CommandType.StoredProcedure;

Ve většině případů vracející tabulku parametry mají ekvivalentní nebo vyšší výkon než jinými dávkování postupy. Parametry vracející tabulku, jsou často konjunktivu, protože jsou flexibilnější než další možnosti. Jinými postupy, například SQL hromadné kopírování, například povolit pouze vložení nové řádky. Ale s parametry vracející tabulku můžete logiky v uložená procedura určit, které řádky jsou aktualizace a které jsou vloží. Typ tabulky můžete také upravit tak, aby obsahují "Operace" sloupec, který označuje, zda zadaný řádek by měl být vložen, aktualizace nebo odstranit.

Následující tabulka zobrazuje výsledky testů ad-hoc pro použití vracející tabulku parametrů v milisekundách.

| Operace | Místní na Azure ([ms)  | Azure stejné datacentra ([ms) |
|---|---|---|
| 1 | 124 | 32 |
| 10 | 131 | 25 |
| 100 | 338 | 51 |
| 1 000 | 2615 | 382 |
| 10000 | 23830 | 3586 |

>[AZURE.NOTE] Výsledky nejsou ukazatele. Zobrazuje [upozornění o načasování výsledky v tomto tématu](#note-about-timing-results-in-this-topic).

Zvýšení výkonu z dávky je okamžitě zřejmé. Ve sloupci předchozí sekvenční podmínka trvala 1000 operace 129 sekund mimo datacentra a 21 sekund z v rámci datacentra. Ale s parametry vracející tabulku 1000 operace trvat pouze 2.6 sekund, než mimo datacentra a 0,4 sekund v rámci datacentra.

Další informace o parametry vracející tabulku najdete v článku [Table-Valued parametry](https://msdn.microsoft.com/library/bb510489.aspx).

### <a name="sql-bulk-copy"></a>Kopírovat hromadné SQL
Kopírovat hromadné SQL je další způsob, jak vložit velké objemy dat do cílové databáze. .NET aplikace můžete použít třídu **SqlBulkCopy** provádět operace vložení hromadně. **SqlBulkCopy** je podobná funkci nástroj příkazového řádku, **Bcp.exe**nebo jazyce Transact-SQL příkazu **Vložit HROMADNĚ**. Následující příklad ukazuje, jak hromadně kopírovat řádky ve zdroji **datové tabulky**, tabulka serveru SQL do cílové tabulky.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        using (SqlBulkCopy bulkCopy = new SqlBulkCopy(connection))
        {
            bulkCopy.DestinationTableName = "MyTable";
            bulkCopy.ColumnMappings.Add("mytext", "mytext");
            bulkCopy.ColumnMappings.Add("num", "num");
            bulkCopy.WriteToServer(table);
        }
    }

Existuje několik případech, kdy hromadné kopírování upřednostňované přes vracející tabulku parametry. Podívejte se na porovnávací tabulku vracející tabulku parametrů versus HROMADNÉ vložení operace v tématu [Table-Valued parametry](https://msdn.microsoft.com/library/bb510489.aspx).

Následující výsledky testů ad-hoc zobrazit výkon dávky s **SqlBulkCopy** v milisekundách.

| Operace | Místní na Azure ([ms)  | Azure stejné datacentra ([ms) |
|---|---|---|
| 1 | 433 | 57 |
| 10 | 441 | 32 |
| 100 | 636 | 53 |
| 1 000 | 2535 | 341 |
| 10000 | 21605 | 2737 |

>[AZURE.NOTE] Výsledky nejsou ukazatele. Zobrazuje [upozornění o načasování výsledky v tomto tématu](#note-about-timing-results-in-this-topic).

Použití parametrů vracející tabulku v menší velikosti dávku outperformed **SqlBulkCopy** předmětu. Však **SqlBulkCopy** provést 12 – 31 % rychlejší než vracející tabulku parametry testů 1 000 a 10 000 řádků. Podobně jako vracející tabulku parametry **SqlBulkCopy** je vhodný pro dávkový vloží, zejména v porovnání s výkon-dávce operací.

Další informace o hromadné kopírování v ADO.NET najdete v článku [Hromadné operace kopírování na serveru SQL Server](https://msdn.microsoft.com/library/7ek5da1a.aspx).

### <a name="multiple-row-parameterized-insert-statements"></a>Více řádků s parametry vložit příkazy
Jedna alternativa malých dávkách je vytvořit velké parametry příkaz vložení, vložení několika řádků. Následující příklad ukazuje tento postup.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        string insertCommand = "INSERT INTO [MyTable] ( mytext, num ) " +
            "VALUES (@p1, @p2), (@p3, @p4), (@p5, @p6), (@p7, @p8), (@p9, @p10)";
    
        SqlCommand cmd = new SqlCommand(insertCommand, connection);
    
        for (int i = 1; i <= 10; i += 2)
        {
            cmd.Parameters.Add(new SqlParameter("@p" + i.ToString(), "test"));
            cmd.Parameters.Add(new SqlParameter("@p" + (i+1).ToString(), i));
        }
    
        cmd.ExecuteNonQuery();
    }
 

V tomto příkladu je určen pro zobrazení základní koncept. Další reálné situace by opakovaně prochází požadované entity vytvářet řetězce dotazu a parametry příkazu současně. Jste omezené z celkové hodnoty parametrů 2100 dotazů, toto nastavení omezuje počet řádků, zpracovaných tímto způsobem.

Následující výsledky testů ad-hoc zobrazujícího výkon tohoto typu příkaz insert v milisekundách.

| Operace | Parametry vracející tabulku ([ms) | Vložení jednoho údajů ([ms) |
|---|---|---|
| 1 | 32 | 20 |
| 10 | 30 | 25 |
| 100 | 33 | 51 |

>[AZURE.NOTE] Výsledky nejsou ukazatele. Zobrazuje [upozornění o načasování výsledky v tomto tématu](#note-about-timing-results-in-this-topic).

Tento postup může být trochu rychlejší u listů, které jsou menší než 100 řádků. Zlepšení je sice malé, tento postup je jinou možnost, která může fungovat i v případě dané aplikace.

### <a name="dataadapter"></a>DataAdapter
**DataAdapter** předmětu umožňuje změnit objekt **datovou sadu** a odešlete změny při vložení, aktualizovat a odstraňovat operace. Pokud používáte **DataAdapter** tímto způsobem, je důležité mít na paměti, že samostatné volání pro každou různých operaci. Aby se zvýšil výkon, použijte vlastnost **UpdateBatchSize** počty operacích, které by měl být dávce najednou. Další informace najdete v tématu [Provedení dávku operace pomocí DataAdapters](https://msdn.microsoft.com/library/aadf8fk2.aspx).

### <a name="entity-framework"></a>Entita framework
Entity Framework aktuálně nepodporuje dávky. Různé vývojáři v rámci komunity jste se pokusili ukazují řešení, například přepsat metodu **SaveChanges** . Ale řešení se obvykle složité vlastní aplikace a datového modelu. O této funkci žádosti má projekt codeplex Entity Framework aktuálně stránku diskuse. Tato diskuse zobrazíte, zobrazit [Poznámky ze schůzky návrh – 2 srpen 2012](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).

### <a name="xml"></a>XML
Pro dokončení jsme pocit, že je důležité si o XML jako dávkování strategie. Použití jazyka XML má však žádné výhody nad jiné metody a několik nevýhody. Postup je podobný parametry vracející tabulku, ale souboru XML nebo řetězec předána uložená procedura místo tabulku definované uživatelem. Uložená procedura rozdělí příkazů v uložená procedura.

Existuje několik nevýhody tento postup:

- Práce s jazykem XML může být náročný a chyby chybám.
- Analýza XML v databázi lze procesoru vyžadující značnou.
- Ve většině případů tento způsob je nižší než vracející tabulku parametry.

Použití jazyka XML pro dotazy dávka se nedoporučuje, z těchto důvodů.

## <a name="batching-considerations"></a>Dávkové co byste měli zvážit
V následujících částech obsahují další pokyny pro použití dávky v aplikacích databáze SQL.

### <a name="tradeoffs"></a>Poměry
V závislosti na vaší architektura dávky může zahrnovat vytvářenou výkon a odolnost proti chybám. Zvažte například situace, kdy vaše role neočekávaně havaruje. Pokud dojde ke ztrátě jeden řádek dat dopad je menší než dopad ztráty velké dávku neodeslané řádků. Když vyrovnávací paměť řádky před odesláním do databáze v určeného časového intervalu je větší rizika.

Z důvodu tento kompromis zjistit typ operace tuto dávku můžete. Dávkové více agresivní (větší listy a delší dobu windows) s daty, která je menší kritické.

### <a name="batch-size"></a>Velikost dávky
Podle testů se obvykle žádné výhody rozdělení velkých listů do menší bloky. Ve skutečnosti tohoto pododdílu často za následek nižší výkon než odesílání velkých jedné dávce. Zvažte například situace místo, kam chcete vložit 1 000 řádků. Následující tabulka uvádí, jak dlouho trvá používat vracející tabulku parametry k vložení řádků 1000 při rozdělit menší listy.

| Velikost dávky | Iterací | Parametry vracející tabulku ([ms) |
| -------- | --- | --- |
| 1 000 | 1 | 347 |
| 500 | 2 | 355 |
| 100 | 10 | 465 |
| 50 | 20 | 630 |

>[AZURE.NOTE] Výsledky nejsou ukazatele. Zobrazuje [upozornění o načasování výsledky v tomto tématu](#note-about-timing-results-in-this-topic).

Uvidíte, že je nejlepší možný výkon 1 000 řádků můžou odeslat v celém dokumentu. V jiné testy (tady to není vidět) byla malé výkonu zisk rozdělí dva listy 5000 dávky 10000 řádku. Ale schématu tabulky pro tyto testy relativně jednoduché, tak na určitá data a velikosti dávka pro ověření tato zjištění by měl provádění testů.

Jiné faktor vzít v úvahu je, že pokud celkové dávku příliš rozrostla, SQL databáze může omezení a odmítnout potvrďte dávku. Nejlepších výsledků dosáhnete otestujte konkrétní nefunguje k určení, zda je ideální dávku velikost. Velikost dávky konfigurovatelné za běhu povolit rychlé úpravy založené na výkon nebo chyby.

Nakonec s riziky spojenými s dávky zůstatek velikost listu. Pokud došlo k chybám přechodná nebo roli nezdaří, zvažte důsledky opakování operace nebo ztráty dat na listu.

### <a name="parallel-processing"></a>Paralelní zpracování
Co když trvaly přístup zmenšení velikosti dávku ale použít více vláken k provedení práce? Znovu testů ukázal, aby několik menší podprocesy listů, obvykle se provádí horší, než jedné dávce větší. Následující test pokusí se vložit řádky 1000 do jednoho nebo více paralelní listy. Tento test ukazuje, jak více listů současně ve skutečnosti snížení výkonu.

| Velikost dávky [iterací] | Dvě vlákna ([ms) | Čtyři vláknech ([ms) | Šest vláknech ([ms) |
| -------- | --- | --- | --- |
| 1 000 [1] | 277 | 315 | 266 |
| 500 [2] | 548 | 278 | 256 |
| 250 [4] | 405 | 329 | 265 |
| 100 [10] | 488 | 439 | 391 |

>[AZURE.NOTE] Výsledky nejsou ukazatele. Zobrazuje [upozornění o načasování výsledky v tomto tématu](#note-about-timing-results-in-this-topic).

Existuje několik možných příčin snížení výkonu z důvodu paralelismus:

- Existuje několik volání souběžné síti místo jedné.
- Více operace u jedné tabulky mohou způsobit konflikty a blokování.
- Existuje režijních nákladů přidružené multithreading.
- Výdajů otevření více připojení převažuje výhodou souběžné zpracování.

Pokud vyberete jiné tabulky nebo databáze, je možné zobrazíte výkonu získat pomocí této strategie. Databáze sharding nebo organizace bude situace tento přístup. Sharding používá více databází a přesměrovává jiná data na každou databázi. Pokud každý malé dávku na jinou databázi, může být provádění operací paralelně efektivnější. Zvýšení výkonu však není dost důležité použít jako základ pro rozhodnutí používat databázi sharding ve vašem řešení.

U některých typů paralelní provádění menší listy můžete mít za následek lepší výkon žádostí o v systému zatížení. V tomto případě Ačkoli je rychlejší zpracuje jedné dávce větší, zpracování více listů současně může být efektivnější.

Pokud používáte paralelního spuštění, zvažte řízení maximální počet pracovních podprocesů. Menší čísla může vést k menší konflikty a rychlejší čas spuštění. Zvažte taky, jestli to umístí na cílové databáze v připojení transakce i další zatížení.

### <a name="related-performance-factors"></a>Faktory výkonu
Typické pokyny na výkon databáze má dopad i na dávky. Můžete například vložit výkon nižší pro tabulky, které obsahují velké primárního klíče nebo mnoho neseskupené indexy.

Používáte-li vracející tabulku parametry uložené procedury, můžete na příkaz **nastavit NOCOUNT ON** na začátku tohoto postupu. Tento příkaz potlačí ENTER počet ovlivněné řádky postupu. Však podle testů využívání **SET NOCOUNT ON** žádný vliv nebo snížení výkonu. Test uložené procedury byl jednoduché s jednoho **Vložení** příkazu z parametru vracející tabulku. Je možné, že by složitější uložené procedury využít tento příkaz. Ale nechcete se předpokládá, že přidání **nastavit NOCOUNT dál** uložené procedury automaticky zlepšuje výkon. Abyste pochopili efekt, otestujte uloženou procedurou s a bez příkazu **SET NOCOUNT ON** .

## <a name="batching-scenarios"></a>Dávkové scénáře
Následující části popisují, jak použití vracející tabulku parametrů v tři scénáře aplikací. První scénář ukazuje, jak vyrovnávací paměť a dávky společně pracovat. Druhý scénář zlepšuje výkon provádění operací seznam podrobnosti v jedné uložená procedura hovoru. Konečný scénář ukazuje, jak používat vracející tabulku parametrů v operaci "UPSERT".

### <a name="buffering"></a>Ukládání
Sice některé scénáře, které jsou zjevných candidate pro dávky jsou mnoho scénáře, které by mohly využít dávky pozdější zpracování. Zpožděné zpracování však také dokumentů větší riziko, že data jsou ztraceny v případě k neočekávané chybě. Je důležité pochopit toto riziko a zvažte důsledky.

Zvažte například webovou aplikaci, která sleduje historii navigace každého uživatele. Na každé stránce žádosti zaujmout aplikace databáze volání pořizovat záznam zobrazení stránky uživatele. Ale vyšší výkon a škálovatelnost lze dosáhnout vyrovnávací paměť navigace činnosti uživatelů a potom odesláním dat do databáze v dávkách. Aktualizace databáze podle uplynulý čas nebo vyrovnávací paměť můžete aktivovat. Pravidla třeba, mohli byste určit, že by měly být dávku zpracovány po 20 sekund nebo když vyrovnávací paměť dosáhne 1 000 položek.

Následující příklad používá [Informace přípony - příjmu](https://msdn.microsoft.com/data/gg577609) zpracuje pamětí události vyvolané sledování předmětu. Když vyrovnávací paměť zaplní nebo dosažení časového limitu, dávku uživatelská data odeslaný k databázi s parametrem vracející tabulku.

Následující třídy NavHistoryData modely navigace podrobnosti uživatele. Obsahuje základní informace, například identifikátor uživatele, adresu URL k nim získat přístup a přístup čas.

    public class NavHistoryData
    {
        public NavHistoryData(int userId, string url, DateTime accessTime)
        { UserId = userId; URL = url; AccessTime = accessTime; }
        public int UserId { get; set; }
        public string URL { get; set; }
        public DateTime AccessTime { get; set; }
    }

Pro ukládání data navigace uživatele k databázi odpovídá třídě NavHistoryDataMonitor. Jsou v ní metodu RecordUserNavigationEntry, která odpovídá zvýšením **OnAdded** událost. Následující kód ukazuje logickou konstruktor používající příjmu můžete vytvořit pozorovatelným kolekci založené na událost. Potom odebírá této pozorovatelným kolekci metodou rezervy. Přetížení Určuje, že vyrovnávací paměť by měly být odeslány každých 20 sekund nebo 1 000 položek.

    public NavHistoryDataMonitor()
    {
        var observableData =
            Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");
    
        observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
    }

Obslužná rutina převede všechny položky pamětí typu vracející tabulku a předá tento typ uložená procedura zpracovávající dávky. Následující kód ukazuje úplnou definici NavHistoryDataEventArgs a třídy NavHistoryDataMonitor.

    public class NavHistoryDataEventArgs : System.EventArgs
    {
        public NavHistoryDataEventArgs(NavHistoryData data) { Data = data; }
        public NavHistoryData Data { get; set; }
    }
    
    public class NavHistoryDataMonitor
    {
        public event EventHandler<NavHistoryDataEventArgs> OnAdded;
    
        public NavHistoryDataMonitor()
        {
            var observableData =
                Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");
    
            observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
        }
    
        public void RecordUserNavigationEntry(NavHistoryData data)
        {    
            if (OnAdded != null)
                OnAdded(this, new NavHistoryDataEventArgs(data));
        }
    
        protected void Handler(IList<EventPattern<NavHistoryDataEventArgs>> items)
        {
            DataTable navHistoryBatch = new DataTable("NavigationHistoryBatch");
            navHistoryBatch.Columns.Add("UserId", typeof(int));
            navHistoryBatch.Columns.Add("URL", typeof(string));
            navHistoryBatch.Columns.Add("AccessTime", typeof(DateTime));
            foreach (EventPattern<NavHistoryDataEventArgs> item in items)
            {
                NavHistoryData data = item.EventArgs.Data;
                navHistoryBatch.Rows.Add(data.UserId, data.URL, data.AccessTime);
            }
    
            using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
            {
                connection.Open();
    
                SqlCommand cmd = new SqlCommand("sp_RecordUserNavigation", connection);
                cmd.CommandType = CommandType.StoredProcedure;
    
                cmd.Parameters.Add(
                    new SqlParameter()
                    {
                        ParameterName = "@NavHistoryBatch",
                        SqlDbType = SqlDbType.Structured,
                        TypeName = "NavigationHistoryTableType",
                        Value = navHistoryBatch,
                    });
    
                cmd.ExecuteNonQuery();
            }
        }
    }

Použití tohoto vyrovnávací třídy, aplikace vytvoří statický NavHistoryDataMonitor objekt. Pokaždé, když uživatel na stránce aplikace volá metodu NavHistoryDataMonitor.RecordUserNavigationEntry. Vyrovnávací logickou vytvářejí starat o odeslání tyto položky do databáze v dávkách.

### <a name="master-detail"></a>Hlavní podrobností
Vracející tabulku parametrů jsou užitečné pro jednoduché scénáře vložit. Však může být náročnější vloží dávku, které zahrnují více než jednu tabulku. Scénář "seznam a podrobnosti" je příkladem. V hlavním tabulce jsou uvedeny primární entity. Jeden nebo více tabulek podrobností obsahují další údaje o entity. V tomto scénáři vynutit relace cizích klíčů relaci podrobnosti a jedinečnou předlohy entitu. Zvažte zjednodušenou verzí PurchaseOrder tabulku a jeho přidružený OrderDetail. Následující jazyce Transact-SQL vytvoří tabulku PurchaseOrder se čtyřmi sloupci: OrderID, DatumObjednávky, CustomerID a stav.

    CREATE TABLE [dbo].[PurchaseOrder](
    [OrderID] [int] IDENTITY(1,1) NOT NULL,
    [OrderDate] [datetime] NOT NULL,
    [CustomerID] [int] NOT NULL,
    [Status] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrder] 
    PRIMARY KEY CLUSTERED ( [OrderID] ASC ))

Každé objednávce obsahuje jeden nebo více nákupy výrobků. Tyto informace se zaznamenává v tabulce PurchaseOrderDetail. Následující jazyce Transact-SQL vytvoří tabulku PurchaseOrderDetail pět sloupců: KódObjednávky OrderDetailID, Idproduktu, JednotkováCena a OrderQty.

    CREATE TABLE [dbo].[PurchaseOrderDetail](
    [OrderID] [int] NOT NULL,
    [OrderDetailID] [int] IDENTITY(1,1) NOT NULL,
    [ProductID] [int] NOT NULL,
    [UnitPrice] [money] NULL,
    [OrderQty] [smallint] NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrderDetail] PRIMARY KEY CLUSTERED 
    ( [OrderID] ASC, [OrderDetailID] ASC ))

OrderID sloupec v tabulce PurchaseOrderDetail musí odkazovat objednávku z tabulky PurchaseOrder. Následující definici cizí klíč zajišťuje toto omezení.

    ALTER TABLE [dbo].[PurchaseOrderDetail]  WITH CHECK ADD 
    CONSTRAINT [FK_OrderID_PurchaseOrder] FOREIGN KEY([OrderID])
    REFERENCES [dbo].[PurchaseOrder] ([OrderID])

K použití vracející tabulku parametry, musíte mít jeden typ uživatelem definovaných tabulky pro každou cílovou tabulku.

    CREATE TYPE PurchaseOrderTableType AS TABLE 
    ( OrderID INT,
      OrderDate DATETIME,
      CustomerID INT,
      Status NVARCHAR(50) );
    GO
    
    CREATE TYPE PurchaseOrderDetailTableType AS TABLE 
    ( OrderID INT,
      ProductID INT,
      UnitPrice MONEY,
      OrderQty SMALLINT );
    GO

Potom definujte uložené procedury, která přijímá tabulek z těchto typů. Tento postup umožňuje aplikaci místně dávkové sadu objednávky a Rozpis objednávek v jediné volání. Následující jazyce Transact-SQL obsahuje deklarace dokončení uložené procedury v tomto příkladu nákupní objednávky.

    CREATE PROCEDURE sp_InsertOrdersBatch (
    @orders as PurchaseOrderTableType READONLY,
    @details as PurchaseOrderDetailTableType READONLY )
    AS
    SET NOCOUNT ON;
    
    -- Table that connects the order identifiers in the @orders
    -- table with the actual order identifiers in the PurchaseOrder table
    DECLARE @IdentityLink AS TABLE ( 
    SubmittedKey int, 
    ActualKey int, 
    RowNumber int identity(1,1)
    );
     
          -- Add new orders to the PurchaseOrder table, storing the actual
    -- order identifiers in the @IdentityLink table   
    INSERT INTO PurchaseOrder ([OrderDate], [CustomerID], [Status])
    OUTPUT inserted.OrderID INTO @IdentityLink (ActualKey)
    SELECT [OrderDate], [CustomerID], [Status] FROM @orders ORDER BY OrderID;
    
    -- Match the passed-in order identifiers with the actual identifiers
    -- and complete the @IdentityLink table for use with inserting the details
    WITH OrderedRows As (
    SELECT OrderID, ROW_NUMBER () OVER (ORDER BY OrderID) As RowNumber 
    FROM @orders
    )
    UPDATE @IdentityLink SET SubmittedKey = M.OrderID
    FROM @IdentityLink L JOIN OrderedRows M ON L.RowNumber = M.RowNumber;
    
    -- Insert the order details into the PurchaseOrderDetail table, 
          -- using the actual order identifiers of the master table, PurchaseOrder
    INSERT INTO PurchaseOrderDetail (
    [OrderID],
    [ProductID],
    [UnitPrice],
    [OrderQty] )
    SELECT L.ActualKey, D.ProductID, D.UnitPrice, D.OrderQty
    FROM @details D
    JOIN @IdentityLink L ON L.SubmittedKey = D.OrderID;
    GO

V tomto příkladu místně definované @IdentityLink tabulka obsahuje skutečné hodnoty KódObjednávky z nově vložených řádků. Tyto identifikátory pořadí se liší od dočasné KódObjednávky hodnoty v @orders a @details vracející tabulku parametry. Z tohoto důvodu @IdentityLink tabulky připojí KódObjednávky hodnoty z @orders parametr skutečné hodnoty ID objednávky pro nové řádky v tabulce PurchaseOrder. Po dokončení tohoto kroku @IdentityLink tabulky mohou usnadnit vložení Rozpis objednávek s skutečné ID objednávky, který splňuje omezení cizího klíče.

Uložená procedura lze z kód nebo dalším voláním jazyce Transact-SQL. Naleznete v části vracející tabulku parametry tento papíru, například kód. Následující jazyce Transact-SQL ukazuje, jak chcete zavolat sp_InsertOrdersBatch.

    declare @orders as PurchaseOrderTableType
    declare @details as PurchaseOrderDetailTableType
    
    INSERT @orders 
    ([OrderID], [OrderDate], [CustomerID], [Status])
    VALUES(1, '1/1/2013', 1125, 'Complete'),
    (2, '1/13/2013', 348, 'Processing'),
    (3, '1/12/2013', 2504, 'Shipped')
    
    INSERT @details
    ([OrderID], [ProductID], [UnitPrice], [OrderQty])
    VALUES(1, 10, $11.50, 1),
    (1, 12, $1.58, 1),
    (2, 23, $2.57, 2),
    (3, 4, $10.00, 1)
    
    exec sp_InsertOrdersBatch @orders, @details

Toto řešení umožňuje každý dávku použít sadu hodnot ID objednávky, které začít od 1. Tyto dočasné hodnoty KódObjednávky popisují vztahy na listu, ale skutečné hodnoty KódObjednávky jsou určeny v době operace vložení. Můžete spustit stejné příkazy v předchozím příkladu opakovaně a vytvoření jedinečných objednávek v databázi. Z tohoto důvodu můžete do ní přidat další kód nebo databáze logiky, který brání duplicitní objednávky při použití tohoto dávky postup.

Tento příklad ukazuje, můžete dávce i složitější databáze operací, jako je operace podrobností pomocí parametrů vracející tabulku.

### <a name="upsert"></a>UPSERT
Jiné dávkování scénář se týká současně aktualizací existujících řádků a vkládání nových řádků. Tuto operaci je někdy označovány jako operace "UPSERT" (aktualizace + insert). Příkaz Sloučit místo samostatné volání můžete VKLÁDAT a aktualizovat, je nejvhodnější k tomuto úkolu. Příkaz Sloučit můžete provádět vybranými položkami vložit operace a aktualizace v jediné volání.

Parametry vracející tabulku lze pomocí příkazu Sloučit provést aktualizace a vloží. Zvažte například zjednodušené zaměstnance tabulku, která obsahuje následující sloupce: EmployeeID jméno, příjmení, SocialSecurityNumber:

    CREATE TABLE [dbo].[Employee](
    [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [SocialSecurityNumber] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_Employee] PRIMARY KEY CLUSTERED 
    ([EmployeeID] ASC ))
 
V tomto příkladu můžete skutečnost, že je SocialSecurityNumber jedinečné ke sloučení více zaměstnanců. Nejprve vytvořte typ uživatelem definovaných tabulky:

    CREATE TYPE EmployeeTableType AS TABLE 
    ( Employee_ID INT,
      FirstName NVARCHAR(50),
      LastName NVARCHAR(50),
      SocialSecurityNumber NVARCHAR(50) );
    GO

Dále vytvořte uložené procedury nebo zadat kód, který používá příkaz Sloučit provést aktualizaci a vložit. V následujícím příkladu příkazu Sloučit na parametr vracející tabulku @employees, typu EmployeeTableType. Obsah @employees tabulky nejsou vidět tady.

    MERGE Employee AS target
    USING (SELECT [FirstName], [LastName], [SocialSecurityNumber] FROM @employees) 
    AS source ([FirstName], [LastName], [SocialSecurityNumber])
    ON (target.[SocialSecurityNumber] = source.[SocialSecurityNumber])
    WHEN MATCHED THEN 
    UPDATE SET
    target.FirstName = source.FirstName, 
    target.LastName = source.LastName
    WHEN NOT MATCHED THEN
       INSERT ([FirstName], [LastName], [SocialSecurityNumber])
       VALUES (source.[FirstName], source.[LastName], source.[SocialSecurityNumber]);

Další informace najdete v tématu dokumentace a příklady pro příkazu Sloučit. Přestože lze provést stejnou práci v vícekrokové uložené volání procedur s oddělte vložení a operace aktualizace, příkaz Sloučit je efektivnější. Kódu databáze můžete taky vytvořit jazyce Transact-SQL hovory, které pomocí příkazu Sloučit přímo bez nutnosti dvou volání databáze pro vkládání a aktualizace.

## <a name="recommendation-summary"></a>Souhrnné doporučení

Následující seznam obsahuje souhrn dávkování doporučení popisované v tomto tématu:

- Umožňuje vyrovnávací paměť a dávky zvýšit výkon a škálovatelnost SQL databáze aplikací.
- Kompromisy mezi dávky/vyrovnávací paměť a odolnost proti chybám. Během selhání role riziko ztráty nezpracovaná dávku důležitých podnikových dat může náročnější výkonu z dávky.
- Odeslání zachovat všechny volání do databáze v rámci jedné datacentra zmenšit latence.
- Pokud se rozhodnete jednoho dávkování postup, vracející tabulku parametry nabízejí nejlepší výkon a flexibilitu.
- Nejrychlejší vložení výkon postupujte podle následujících obecných pokynů ale testovat nefunguje:
    - Použití typu single < 100 řádků s parametry příkaz Vložit.
    - < 1 000 řádků pomocí parametrů vracející tabulku.
    - Pro > = 1 000 řádků, použijte SqlBulkCopy.
- Pro aktualizace a operace odstraňování, vracející tabulku parametrů pomocí uložená procedura použití logických operátorů, která určuje správné operace v jednotlivých řádcích tabulky parametr.
- Pokyny k dávku velikost:
    - Použijte největší dávku formáty, které vyhovují aplikace a obchodní požadavky.
    - Zůstatek zvýšení výkonu velkých listů s riziky spojenými s dočasné nebo katastrofické k chybám. Co je důsledek opakování a ztrátu dat na listu? 
    - Otestujte nejdelší dávka pro ověření, že databáze SQL není odmítne.
    - Vytvoření nastavení této ovládací prvek dávky, jako je velikost dávky nebo vyrovnávací časového intervalu. Tato nastavení poskytují možnosti. Dávkování chování v výrobní můžete změnit bez znovu nasazení cloudovou službu.
- Vyhněte se paralelní provádění listy, které fungují na jedné tabulky do jedné databáze. Pokud se rozhodnete rozdělíte jedné dávce mezi více vláken kolegy, spusťte testy a určete ideální počet vláknech. Po neurčeným mezní hodnota více vláknech bude snížení výkonu spíše než zvětšit.
- Zvažte vyrovnávací paměť o velikosti a času jako způsob provádění dávky další scénáře.

## <a name="next-steps"></a>Další kroky

Tento článek se zaměřuje na způsob návrhu databáze a kódování postupy týkající se dávky můžete zlepšit aplikaci výkon a škálovatelnost. Ale jediným faktorem strategie celkové. Další způsoby, jak zlepšit výkon a škálovatelnost v tématu [pokyny databáze SQL Azure výkonu pro jednu databáze](sql-database-performance-guidance.md) a [cena a výkonu pro fond pružná databáze](sql-database-elastic-pool-guidance.md).
