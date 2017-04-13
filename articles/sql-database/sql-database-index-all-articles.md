<properties
    pageTitle="Všech témat služby SQL databáze | Microsoft Azure"
    description="Tabulka všech témat pro službu Azure název databáze SQL, která jsou na http://azure.microsoft.com/documentation/articles/, název a popis."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="genemi"/>


# <a name="all-topics-for-azure-sql-database-service"></a>Všech témat služby databáze SQL Azure

Toto téma obsahuje seznam všech téma, které slouží k použití přímo do služby **Databáze SQL** Azure. Hledat tuto webovou stránku klíčových slov pomocí **Kombinace kláves Ctrl + F**, najdete v tématech aktuální zájmu.




## <a name="new"></a>Nový

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 1 | [Daxko/CSI používá Azure urychlit jeho cyklu vývoje a zlepšit jeho služeb zákazníkům a výkon](sql-database-implementation-daxko.md) | Další informace o použití databáze SQL Daxko/CSI urychlit jeho cyklu vývoje a zlepšit jeho služeb zákazníkům a výkon |
| 2 | [Azure poskytuje GEP globální rozsah a větší efektivitu](sql-database-implementation-gep.md) | Další informace o použití databáze SQL GEP k dosažení globálnější zákazníků a k dosažení větší efektivity |
| 3 | [S Azure má SnelStart rychle rozbalená jeho podnikové sazbou 1 000 nové databáze SQL Azure, za měsíc](sql-database-implementation-snelstart.md) | Další informace o použití databáze SQL SnelStart rychle rozbalit jeho podnikové rychlostí 1 000 nové databáze SQL Azure, za měsíc |
| 4 | [Umbraco používá databáze SQL Azure rychle měřítko a poskytování služeb pro tisíce tenantům v cloudu](sql-database-implementation-umbraco.md) | Další informace o použití databáze SQL Umbraco rychle zřízení a měřítko služby pro tisíce tenantům v cloudu |
| 5 | [Správa databáze Azure SQL pomocí prostředí PowerShell](sql-database-manage-powershell.md) | Azure SQL databáze Správa prostřednictvím Powershellu. |
| 6 | [Podpora SSMS MFA Azure AD pomocí databáze SQL a SQL datový sklad](sql-database-ssms-mfa-authentication.md) | Použití více promítnou ověřování v SSMS pro databáze SQL a SQL datový sklad. |
| 7 | [Vysvětlení jednotek databázové transakce (DTUs) a pružná jednotek databázové transakce (eDTUs)](sql-database-what-is-a-dtu.md) | Principy jaké Azure SQL databáze aplikace jednotku transakce je. |


## <a name="updated-articles-sql-database"></a>Aktualizované články, databáze SQL

V této části jsou uvedeny články, které byly aktualizovány naposledy, kde aktualizace proběhla velká nebo významný. Pro každý aktualizovaný článek se zobrazí hrubé fragment kódu přidané markdown textu. V rozsahu kalendářních dat **2016-08-22** **2016 10 05**aktualizovaly se tyto články.

| &nbsp; | Článek | Aktualizované text, fragment kódu | Aktualizace |
| --: | :-- | :-- | :-- |
| 8 | [Správa databáze Azure SQL pomocí prostředí PowerShell](sql-database-command-line-tools.md) | Vytvoření skupiny prostředků pro naše databáze SQL a související materiály Azure s rutinu New-AzureRmResourceGroup (https://msdn.microsoft.com/library/azure/mt759837.aspx). *$resourceGroupName = "resourcegroup1" $resourceGroupLocation = "northcentralus" nový AzureRmResourceGroup – název $resourceGroupName – umístění $resourceGroupLocation* Další informace najdete v tématu přes Azure pomocí Správce prostředků Azure (... / prostředí powershell azure zdroje manager.md). Ukázka skriptu najdete v článku Vytvoření databáze SQL skript Powershellu (sql databáze get Začínáme – powershell.md create-a-sql-database-powershell-script). **Vytvoření databáze SQL serveru** Vytvoření databáze SQL serveru s rutinu New-AzureRmSqlServer (https://msdn.microsoft.com/library/azure/mt603715.aspx). Nahraďte *server1* název svého serveru. Názvy serverů musí být jedinečný na všech serverech databáze SQL Azure. Pokud již není přijato název serveru, dojde k chybě. Tento příkaz může trvat několik minut. Resou | 2016-09-14 |
| 9 | [Zkopírujte databázi Azure SQL pomocí prostředí PowerShell](sql-database-copy-powershell.md) |   SQL databáze zdroj (stávající databázi zkopírovat) – $sourceDbName = "ZR1" $sourceDbServerName = "server1" $sourceDbResourceGroupName = kopie databáze SQL "rg1" (nové databáze vytvořit) – $copyDbName = "db1_copy" $copyDbServerName = "server2" $copyDbResourceGroupName = "rg2" kopii databáze na stejný server – nový AzureRmSqlDatabaseCopy - ResourceGroupName $sourceDbResourceGroupName - webu název serveru $sourceDbServerName – název databáze $sourceDbName - CopyDatabaseName $copyDbName kopírování databáze do různých server – nový AzureRmSqlDatabaseCopy - ResourceGroupName $sourceDbResourceGroupName - webu název serveru $sourceDbServerName – název databáze $sourceDbName - CopyResourceGroupName $copyDbResourceGroupName - CopyServerName $copyDbServerName - CopyDatabaseName $copyDbName zkopírujte databáze do fondu pružná databáze – $poolName = $ New AzureRmSqlDatabaseCopy - ResourceGroupName "pool1" sourceDbResourceGroupName - webu název serveru $sourceDbServerName – název databáze $sourceDbName - CopyReso | 2016-09-08 |
| 10 | [Vytvoření fondu pružná databáze s C#](sql-database-elastic-pool-create-csharp.md) |   Console.WriteLine ("Server brány firewall...");  FirewallRuleGetResponse fwr = CreateOrUpdateFirewallRule (_sqlMgmtClient _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);  Console.WriteLine ("brány firewall serveru:" + fwr. FirewallRule.Id);  Console.WriteLine ("pružná fondu...");  ElasticPoolCreateOrUpdateResponse epr = CreateOrUpdateElasticDatabasePool (_sqlMgmtClient _resourceGroupName, _serverName, _poolName, _poolEdition, _poolDtus, _databaseMinDtus, _databaseMaxDtus);  Console.WriteLine ("pružná fondu:" + epr. ElasticPool.Id);  Console.WriteLine("Database...");  DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase (_sqlMgmtClient _resourceGroupName, _serverName, _databaseName, _poolName);  Console.WriteLine ("databáze:" + dbr. Database.Id);  Console.WriteLine ("stisknutím libovolné klávesy dál...");  Console.ReadKey();  } statické CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupNa ResourceGroup | 2016-09-14 |
| 11 | [Skript Powershellu pro identifikaci databází vhodné pro fond pružná databáze](sql-database-elastic-pool-database-assessment-powershell.md) | **Pro pružná fond databáze kandidátů jsme vyloučit předlohy a databází, které jsou již fond. Můžete přidat další databáze do seznamu vyloučení dole podle potřeby** $ListOfDBs = Get-AzureRmSqlDatabase - webu název serveru $servername. Split('.') 0 - ResourceGroupName $ResourceGroupName / Where objekt {$_. Název databáze - notin ("předlohy") - a $_. CurrentServiceLevelObjectiveName - notin ("ElasticPool")- a $_. CurrentServiceObjectiveName - notin ("ElasticPool")} $outputConnectionString = "zdroj dat = $outputServerName; integrované zabezpečení = NEPRAVDA; počátečního katalogu = $outputdatabaseName; Id uživatele = $outputDBUsername; Heslo = $outputDBpassword "$destinationTableName ="resource_stats_output" **vytvořte tabulku v databázi výstup metriky kolekce** $sql =" Pokud klíčová slova NOT EXISTS (vyberte * z sys.objects WHERE object_id = OBJECT_ID(N'$($destinationTableName)') a zadejte hodnotu do (N'U ")) $($destinationTableName) POČÁTEČNÍHO vytvořit tabulku (název_databáze varchar(128) slo varchar(20), end_time data a času, avg_cpu plovoucí, avg_ | 2016-09 – 29 |
| 12 | [Databáze SQL kurz: vytvoření databáze SQL v minutách pomocí portálu Azure](sql-database-get-started.md) |   ! Nové databáze sql 1 (. / media/sql-database-get-started/sql-database-new-database-1.png) 3. Klikněte na **Databáze SQL** otevřete zásuvné databáze SQL. Obsah na tomto zásuvné se liší v závislosti na počtu předplatné a existující objekty (například stávající servery).  ! Nové databáze sql 2 (. / media/sql-database-get-started/sql-database-new-database-2.png) 4. Do pole **název databáze** zadejte název pro první databáze – například "Moje databáze". Zelená značka zaškrtnutí označuje, že jste zadali platný název.  ! Nové databáze sql 3 (. / media/sql-database-get-started/sql-database-new-database-3.png) 5. Pokud máte víc předplatných vyberte předplatné. 6. V části **Skupina zdroje**klikněte na **vytvořit nový** a zadejte název pro první skupina zdroje – například "my-– skupina zdroje". Zelená značka zaškrtnutí označuje, že jste zadali platný název.  ! Nové databáze sql 4 (. / media/sql-database-get-started/sql-database-new-database-4.png) 7. V části **Vyberte zdroj výsledků* | 2016-09-08 |
| 13 | [Zkuste SQL databáze: Použití C# k vytvoření databáze SQL s knihovnou SQL databáze pro .NET](sql-database-get-started-csharp.md) | **Vytvořit hlavní k přístupu k prostředkům službu** Tento skript Powershellu vytvoří aplikace Active Directory (AD) a hlavní služby, potřebujeme ověření naše C aplikace. Skript výstupy hodnoty, které potřebujeme pro v předchozím příkladu C. Podrobné informace najdete v článku pomocí Azure vytvořit hlavní název služby přístupu k prostředkům (... / zdrojů – skupiny – ověření služby – principal.md).  Přihlaste se k Azure.  Přidání AzureRmAccount Pokud máte víc předplatných, zrušte komentář a je nastavena pro předplatné, které chcete pracovat s.  $subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}" Set AzureRmContext - SubscriptionId $subscriptionId poskytnout tyto hodnoty pro novou aplikaci AAD.  $appName je zobrazovaný název aplikace, musí být jedinečný v adresáři.  $uri nemusí být skutečné identifikátor uri.  $secret je heslo, které vytvoříte.  $appName = "{– název aplikace}" $uri = "http://{app-name}" $secret = "{aplikace hesla}" vytvořit v aplikaci AAD $azureAdApplication = nový AzureRmADApp | 2016-09-23 |
| 14 | [Správa databáze SQL Azure pomocí portálu Azure](sql-database-manage-portal.md) | Chcete-li zobrazit nebo aktualizovat nastavení databáze, klikněte na požadované nastavení na zásuvné databáze SQL:! Nastavení databáze SQL (. / media/sql-database-manage-portal/settings.png) **jak najít databáze plně kvalifikovaný název serveru SQL Server?** Pokud chcete zobrazit název svého serveru databáze, klikněte na **Přehled** na zásuvné **databáze SQL** a poznamenejte si název serveru:! Nastavení databáze SQL (. / media/sql-database-manage-portal/server-name.png) **jak spravovat pravidla brány firewall pro řízení přístupu k Moje serveru SQL server a databázi?** Zobrazení, vytvoření nebo aktualizace pravidel brány firewall, klikněte na **nastavení brány firewall serveru** na zásuvné **databáze SQL** . Další informace najdete v tématu Konfigurace brány firewall na úrovni serveru pravidlo databáze SQL Azure pomocí portálu Azure (sql databáze – konfigurace-brány firewall-settings.md). ! Brána firewall pravidla (. / media/sql-database-manage-portal/sql-database-firewall.png) **jak změnit svůj SQL databáze služby osy úroveň výkonu a?** Aktualizace služby osy nebo výkonu úroveň SQL databáze, klikněte na ** ceny osy (ne | 2016-09-20 |





## <a name="get-started"></a>Začínáme

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 15 | [Vytvoří více klienta aplikace s databáze Azure SQL pomocí izolace a efektivity](sql-database-build-multi-tenant-apps.md) | Zjistěte, jak databáze SQL vytvoří více klienta aplikace |
| 16 | [Připojení k databázi SQL s SQL Server Management Studio a spusťte dotaz T-SQL ukázka](sql-database-connect-query-ssms.md) | Informace o připojení k databázi SQL v Azure pomocí služby SQL Server Management Studio (SSMS). Spusťte dotaz vzorku pomocí jazyce Transact-SQL (T-SQL). |
| 17 | [Připojení k databázi SQL pomocí .NET (C#)](sql-database-develop-dotnet-simple.md) | Použití ukázkový kód v této rychle začít vytvářet aplikace moderní s C# and podporovaným výkonné relační databáze v cloudu pomocí databáze SQL Azure. |
| 18 | [Vytvoření nového fondu pružná databáze pomocí portálu Azure](sql-database-elastic-pool-create-portal.md) | Jak přidat fond scalable pružná databáze SQL databáze konfigurace pro snadnější správu a sdílení přes mnoho databází zdrojů. |
| 19 | [Prozkoumání výukové programy pro databáze Azure SQL](sql-database-explore-tutorials.md) | Další informace o databáze SQL funkcím a možnostem |
| 20 | [Databáze SQL kurz: vytvoření databáze SQL v minutách pomocí portálu Azure](sql-database-get-started.md) | Zjistěte, jak nastavit logické databáze SQL serveru, serveru pravidlo brány firewall, databáze SQL a ukázková data. Také Naučte se spojit s klientských nástrojích konfigurace uživatelů a nastavení brány firewall pravidla databáze. |
| 21 | [Databáze Azure SQL zabezpečuje a chrání](sql-database-helps-secures-and-protects.md) | Dozvíte se, jak pomáhá databáze SQL zabezpečení a ochrana |
| 22 | [Databáze Azure SQL učí &amp; přizpůsobuje](sql-database-learn-and-adapt.md) | Zjistěte, jak databáze SQL učí a přizpůsobuje |
| 23 | [Zvolte obláčkem možnost SQL Server: databáze SQL Azure (PaaS) nebo SQL Server na Azure VMs (IaaS)](sql-database-paas-vs-sql-server-iaas.md) | Zjistěte, kterou možnost SQL Server cloudu vešel aplikace: databáze SQL Azure (PaaS) nebo SQL Server v cloudu na virtuálních počítačích Azure. |
| 24 | [Azure SQL databáze škály rychlé úpravy](sql-database-scale-on-the-fly.md) | Zjistěte, jak databáze SQL přizpůsobí rychlé úpravy |
| 25 | [Prozkoumání Azure SQL databáze řešení Snadné spuštění](sql-database-solution-quick-starts.md) | Další informace o řešení databáze Azure SQL |
| 26 | [Databáze Azure SQL funguje ve vašem prostředí](sql-database-works-in-your-environment.md) | Zjistěte, jak databáze SQL pomáhá, zabezpečuje a chrání |



## <a name="build-an-app"></a>Vytvoření aplikace

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 27 | [Poradce při potížích s Diagnostika a zabránit chybám připojení SQL a přechodná databáze SQL](sql-database-connectivity-issues.md) | Informace o řešení potíží s Diagnostika a zabránit SQL připojení chybu nebo přechodná chybu v databázi SQL Azure.  |
| 28 | [Připojení k databázi SQL pomocí aplikace Visual Studio](sql-database-connect-query.md) | Napište program v jazyce C# k vytváření dotazů a připojení k databázi SQL. Informace o IP adresy připojovací řetězec, zabezpečené přihlášení a bezplatné aplikace Visual Studio. |
| 29 | [Porty za 1433 ADO.NET 4.5 a V12 databáze SQL](sql-database-develop-direct-route-ports-adonet-v12.md) | Připojení klientů z ADO.NET k V12 databáze SQL Azure někdy používat proxy server a interaktivně pracovat přímo s databází. Porty než 1433 stane důležité. |
| 30 | [Kódy chyb SQL pro databáze SQL klientských aplikacích: databáze Chyba připojení i jiných problémů](sql-database-develop-error-messages.md) | Seznamte se s kódy chyb SQL pro klientské aplikace databáze SQL, jako jsou běžné chyby při připojení databáze, problémy kopii databáze a obecné chyby.  |
| 31 | [Připojení k databázi SQL pomocí PHP v systému Windows](sql-database-develop-php-simple.md) | Zobrazuje ukázka PHP programu, který se připojuje k databázi SQL Azure z klienta se systémem Windows a poskytuje odkazy na nezbytné součásti softwaru potřeby klientem. |
| 32 | [Zkuste SQL databáze: Použití C# k vytvoření databáze SQL s knihovnou SQL databáze pro .NET](sql-database-get-started-csharp.md) | Zkuste databáze SQL k vývoji aplikací SQL a C# a vytvoření databáze SQL Azure pomocí C# pomocí knihovnu SQL databáze pro .NET. |
| 33 | [Začínáme s funkcemi JSON v databázi SQL Azure](sql-database-json-features.md) | Databáze SQL Azure umožňuje analýzy, dotaz a formátování dat v JavaScript Object Notation (JSON) zápisu. |
| 34 | [Řešení potíží s připojením k databázi SQL Azure](sql-database-troubleshoot-common-connection-issues.md) | Postup při identifikovat a vyřešit běžné chyby při připojení k databázi SQL Azure. |
| 35 | [Chyba "Databáze na serveru není momentálně neexistuje" při připojení k databázi sql](sql-database-troubleshoot-connection.md) | Poradce při potížích s databáze na serveru není momentálně neexistuje chyby při aplikace připojení k databázi SQL. |



## <a name="develop"></a>Můžete vyvíjet

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 36 | [Získat požadované hodnoty pro ověřování aplikace pro přístup k databázi SQL z kódu](sql-database-client-id-keys.md) | Vytvořte hlavní název služby pro přístup k databázi SQL z kódu. |
| 37 | [Připojení k databázi SQL pomocí jazyka Java JDBC v systému Windows](sql-database-develop-java-simple.md) | Představuje vzorek Java kód, který používáte pro připojení k databázi SQL Azure. Příklad používá JDBC a spustí v klientském počítači Windows. |
| 38 | [Připojení k databázi SQL pomocí Node.js](sql-database-develop-nodejs-simple.md) | Představuje vzorek Node.js kód, který používáte pro připojení k databázi SQL Azure. |
| 39 | [Přehled vývoje databáze SQL](sql-database-develop-overview.md) | Informace o připojení k dispozici knihoven a osvědčené postupy pro připojení k databázi SQL aplikace. |
| 40 | [Připojení k databázi SQL pomocí Python](sql-database-develop-python-simple.md) | Představuje vzorek Python kód, který používáte pro připojení k databázi SQL Azure. |
| 41 | [Připojení k databázi SQL pomocí skutečné](sql-database-develop-ruby-simple.md) | Zobrazit ukázku skutečné kódu spuštění připojení k databázi SQL Azure. |
| 42 | [Začínáme s časový tabulek v databázi Azure SQL](sql-database-temporal-tables.md) | Zjistěte, jak začít s pomocí časový tabulek v databázi SQL Azure. |



## <a name="performance-and-scale"></a>Výkon a měřítka

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 43 | [Konference Advisor databáze SQL](sql-database-advisor.md) | Konference Advisor databáze SQL Azure poskytuje doporučení pro existující databáze SQL, který vám pomůže aktuální výkonu dotazu. |
| 44 | [Konference Advisor databáze SQL Azure portálu](sql-database-advisor-portal.md) | Poradce databáze SQL Azure Azure portálu umožňuje zkontrolovat a provádění doporučení pro existující databáze SQL, který vám pomůže aktuální výkonu dotazu. |
| 45 | [Srovnávací přehled Azure SQL databáze](sql-database-benchmark-overview.md) | Toto téma popisuje srovnávacích databáze SQL Azure měření výkonu databáze SQL Azure. |
| 46 | [Pokud má být použita fond pružná databáze?](sql-database-elastic-pool-guidance.md) | Fond pružná databáze najdete dostupné zdroje, které jsou sdílené skupinou pružná databází. Tento dokument obsahuje pokyny k posouzení vhodnosti pomocí fondu pružná databáze pro skupinu databází. |
| 47 | [Začínáme s pružné databázové nástroje](sql-database-elastic-scale-get-started.md) | Základní vysvětlení funkce nástroje pružná databáze databáze SQL Azure, včetně Snadné spuštění aplikace vzorku. |
| 48 | [Databáze SQL časté otázky](sql-database-faq.md) | Odpovědi na běžné otázky zákazníci zeptejte se cloudu databází a databáze SQL Azure, systému správy relační databáze společnosti Microsoft (RDBMS) a databáze jako službu v cloudu. |
| 49 | [Začínáme s v paměti (verze Preview) do SQL databáze](sql-database-in-memory.md) | Technologií SQL v paměti výrazně zvýšit výkon transakční a technologie pro analýzu úloh. Zjistěte, jak umožní využít výhod těchto technologií. |
| 50 | [Použití v paměti OLTP (verze preview) pro zvýšení výkonu aplikace v databázi SQL](sql-database-in-memory-oltp-migration.md) | Přijmout v paměti OLTP transakční zlepšili v existující databázi SQL. |
| 51 | [Sledování OLTP v paměti úložiště](sql-database-in-memory-oltp-monitoring.md) | Sledování XTP v paměti úložiště a odhadu použít, kapacity; řešení chyby kapacita 41823 |
| 52 | [Provozní úložišti dotazu v databázi Azure SQL](sql-database-operate-query-store.md) | Naučte se pracovat úložišti dotazu v databázi SQL Azure |
| 53 | [Přehled výkonu databáze SQL](sql-database-performance.md) | Databáze SQL Azure obsahuje nástroje pro sledování výkonu vám pomůže identifikovat oblasti, které můžete zlepšit výkon aktuální dotazu. |
| 54 | [Azure SQL databáze a výkonu pro jednu databáze](sql-database-performance-guidance.md) | V tomto článku můžete určit, které vrstvy služeb pro aplikace. Doporučuje také způsoby, jak optimalizovat aplikace k optimálnímu využití databáze SQL Azure. |
| 55 | [Přehled výkonu dotazu databáze Azure SQL](sql-database-query-performance.md) | Sledování výkonu dotazu označuje většina jinými procesoru dotazů pro databázi SQL Azure. |
| 56 | [Změna služby osy a výkonu úrovně (cena za osy) databáze SQL](sql-database-scale-up.md) | Změna vrstvy služeb a úroveň výkonu databáze Azure SQL ukazuje, jak změnit velikost databáze SQL nahoru nebo dolů. Změna ceny osy databáze Azure SQL. |
| 57 | [Sledování výkonu databáze v databázi SQL Azure](sql-database-single-database-monitor.md) | Informace o možnostech sledování databáze pomocí nástroje pro Azure a Správa dynamických zobrazení. |
| 58 | [Tipy optimalizaci výkonu databáze SQL](sql-database-troubleshoot-performance.md) | Tipy pro optimalizace v databázi SQL Azure pomocí hodnocení a zlepšení výkonu. |
| 59 | [Jak používat dávky ke zvýšení výkonu aplikace databáze SQL](sql-database-use-batching-to-improve-performance.md) | Téma obsahuje důkaz této dávkování operací databázi výrazně imroves rychlost a škálovatelnost aplikací databáze SQL Azure. I když tyto dávkování techniky práce pro všechny databáze SQL serveru, na článek se zaměřuje na Azure. |



## <a name="tools-for-scaling-out"></a>Nástroje pro změnu velikosti

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 60 | [Návrh vzorků pro víceklientské SaaS aplikace a databáze SQL Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md) | Tento článek popisuje požadavky a běžných architektura vzorků dat víceklientské databázi, kterou spuštěné v prostředí cloudu vzít do úvahy a různé kompromisy přidružené tyto vzorce. Také vysvětluje, jak databáze SQL Azure, s jejími pružná fondů a pružné nástroje pomoct řešit tyto požadavky způsobem bez narušení zabezpečení. |
| 61 | [Migrace existující databáze do škálování](sql-database-elastic-convert-to-use-elastic-tools.md) | Převedení sharded databází používat pružná databázové nástroje vytvořením shard správce mapy |
| 62 | [Vytváření scalable cloudu databází](sql-database-elastic-database-client-library.md) | Vytvoření scalable .NET databáze aplikace s knihovnou klienta pružná databáze |
| 63 | [Výkonnosti pro správce shard mapy](sql-database-elastic-database-perf-counters.md) | ShardMapManager třídy a data závislá směrování výkonnosti |
| 64 | [Přidání shard pomocí pružná databázové nástroje](sql-database-elastic-scale-add-a-shard.md) | Použití rozhraní API pružná měřítko nové shards přidáte shard nastavení. |
| 65 | [Nasazení služby rozdělit sloučení](sql-database-elastic-scale-configure-deploy-split-and-merge.md) | Rozdělení a sloučení s pružná databázové nástroje |
| 66 | [Závislé směrování dat](sql-database-elastic-scale-data-dependent-routing.md) | Způsob použití třídy ShardMapManager v aplikacích pro .NET pro data závislé na směrování, funkce pružná databáze pro databázi SQL Azure |
| 67 | [Pružná databázové nástroje časté otázky](sql-database-elastic-scale-faq.md) | Nejčastější dotazy k pružná měřítko databáze Azure SQL. |
| 68 | [Pružná Glosář databázové nástroje](sql-database-elastic-scale-glossary.md) | Vysvětlení termínů pro pružná databázové nástroje |
| 69 | [Rozšiřování s databáze SQL Azure](sql-database-elastic-scale-introduction.md) | Service (SaaS) vývojáři softwaru, mohli snadno vytvářet pružná, scalable databází v cloudu pomocí těchto nástrojích |
| 70 | [Přihlašovací údaje pro přístup k knihovnu klienta pružná databáze](sql-database-elastic-scale-manage-credentials.md) | Jak nastavit správnou úroveň v části přihlašovací údaje správce jen pro čtení, pružná databáze aplikací |
| 71 | [Dotazování více shard](sql-database-elastic-scale-multishard-querying.md) | Spouštění dotazů přes shards pomocí klienta knihovny pružná databáze. |
| 72 | [Přesouvání dat mezi rozšířených cloudu databází](sql-database-elastic-scale-overview-split-and-merge.md) | Vysvětluje, jak pracovat s shards a přesunutí dat pomocí vlastního hostovanou službu pomocí pružná databáze rozhraní API. |
| 73 | [Rozšiřování databáze s správce shard mapy](sql-database-elastic-scale-shard-map-management.md) | Jak používat ShardMapManager pružná databáze klienta knihovny |
| 74 | [Konfigurace zabezpečení rozdělit hromadné korespondence](sql-database-elastic-scale-split-merge-security-configuration.md) | Nastavení x409 certifikáty pro šifrování |
| 75 | [Upgrade aplikace pro použití nejnovější knihovny klienta pružná databáze](sql-database-elastic-scale-upgrade-client-library.md) | Upgrade aplikace a knihoven pomocí Nuget |
| 76 | [Pružná databáze klienta do knihovny Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) | Použít knihovnu pružná databáze klienta ale Entity Framework pro kódování databází |
| 77 | [Pružná databáze klienta knihovny pomocí Dapper](sql-database-elastic-scale-working-with-dapper.md) | Pružná databáze klienta knihovny pomocí Dapper. |
| 78 | [Více klienta aplikací s pružná databázové nástroje a řádek úrovně zabezpečení](sql-database-elastic-tools-multi-tenant-row-level-security.md) | Naučte se používat pružná databázové nástroje a zabezpečení na úrovni řádek můžete vytvořit aplikaci osy vysoce scalable dat v databázi SQL Azure, který podporuje více klienta shards. |



## <a name="elastic-jobs"></a>Pružná úlohy

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 79 | [Vytváření a správa škálovanou se databáze SQL Azure pomocí pružná úloh (verze preview)](sql-database-elastic-jobs-create-and-manage.md) | Projděte si postup vytvoření a správa úlohy pružná databáze. |
| 80 | [Začínáme s úloh pružná databází](sql-database-elastic-jobs-getting-started.md) | použití úlohy pružná databáze |
| 81 | [Správa databází rozšířených cloudu](sql-database-elastic-jobs-overview.md) | Ukazuje služby práci pružná databáze |
| 82 | [Vytváření a správa projektů pružná databáze SQL databáze pomocí prostředí PowerShell (verze preview)](sql-database-elastic-jobs-powershell.md) | Prostředí PowerShell sloužící ke správě skupin databáze SQL Azure |
| 83 | [Instalace přehled úloh pružná databáze](sql-database-elastic-jobs-service-installation.md) | Projděte si postup instalace funkci pružná projektu. |
| 84 | [Odinstalace součásti úlohy pružná databáze](sql-database-elastic-jobs-uninstall.md) | Jak odinstalovat nástroje pro správu projektů pružná databáze |



## <a name="elastic-pools"></a>Pružná fondů

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 85 | [Co je Azure pružná fondu?](sql-database-elastic-pool.md) | Správa stovky nebo tisíce databází přes fond. Jeden cena pro sadu jednotky výkonu můžete průběhu fondu. Přesunutí databázích či oddálení bude. |
| 86 | [Vytvoření fondu pružná databáze s C#](sql-database-elastic-pool-create-csharp.md) | Použijte C# databáze vývoj techniky pro vytvoření fondu scalable pružná databáze v databázi SQL Azure, sdílení zdrojů mezi mnoho databází. |
| 87 | [Vytvoření nového fondu pružná databáze pomocí prostředí PowerShell](sql-database-elastic-pool-create-powershell.md) | Zjistěte, jak pomocí Powershellu škálování databáze SQL Azure zdroje vytvořením fond scalable pružná databáze pro správu více databází. |
| 88 | [Skript Powershellu pro identifikaci databází vhodné pro fond pružná databáze](sql-database-elastic-pool-database-assessment-powershell.md) | Fond pružná databáze najdete dostupné zdroje, které jsou sdílené skupinou pružná databází. Tento dokument obsahuje skript Powershellu k posouzení vhodnosti pomocí fondu pružná databáze pro skupinu databází. |
| 89 | [Sledování a správa fond pružná databáze s C#](sql-database-elastic-pool-manage-csharp.md) | Správa fondu pružná databáze databáze SQL Azure pomocí C# databáze vývojářské postupy. |
| 90 | [Sledování a správa fond pružná databáze pomocí portálu Azure](sql-database-elastic-pool-manage-portal.md) | Zjistěte, jak používat portál Azure a předdefinované intelligence databáze SQL spravovat, sledovat a vpravo velikost fond scalable pružná databáze můžete optimalizovat výkon databáze a spravovat náklady. |
| 91 | [Sledování a správa fondu pružná databáze pomocí prostředí PowerShell](sql-database-elastic-pool-manage-powershell.md) | Informace o použití Powershellu ke správě fondu pružná databáze. |
| 92 | [Sledování a správa fondu pružná databáze se jazyce Transact-SQL](sql-database-elastic-pool-manage-tsql.md) | Vytvoření databáze Azure SQL ve fondu pružná pomocí T-SQL. Nebo přejdete datbase a odhlášení z fondů pomocí T-SQL. |
| 93 | [Pružná databáze fondu fakturace a ceny informace](sql-database-elastic-pool-price.md) | Ceny informace specifické pro fondy pružná databáze. |



## <a name="elastic-query"></a>Pružná dotazu

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 94 | [Sestava v cloudu rozšířených databáze (verze preview)](sql-database-elastic-query-getting-started.md) | jak používat křížové databáze databázových dotazů |
| 95 | [Začínáme s více databázových dotazů (svislá dělení) (verze preview)](sql-database-elastic-query-getting-started-vertical.md) | jak pomocí svisle rozdělený databází pružná databázových dotazů |
| 96 | [Vytváření sestav v cloudu rozšířených databáze (verze preview)](sql-database-elastic-query-horizontal-partitioning.md) | jak nastavit pružná dotazů přes vodorovné oddíly |
| 97 | [Azure SQL databáze pružná databázi dotazu přehled (verze preview)](sql-database-elastic-query-overview.md) | Základní informace o funkci pružná dotazu |
| 98 | [Dotaz na cloud databáze s různými schématy (verze preview)](sql-database-elastic-query-vertical-partitioning.md) | jak nastavit křížově databázových dotazů přes svislé oddíly |



## <a name="elastic-transaction"></a>Pružná transakce

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 99 | [Distribuované transakce přes cloudu databází](sql-database-elastic-transactions-overview.md) | Základní informace o pružná databázové transakce s databáze Azure SQL |



## <a name="backup-and-recovery"></a>Zálohování a obnovení

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 100 | [Zálohování databáze SQL](sql-database-automated-backups.md) | Informace o databáze SQL předdefinované zálohování databáze, které vám umožní vrátit zpět databáze SQL Azure předchozí bodu v čase nebo kopírování databáze do nové databáze ve zeměpisná oblast (až 35 dní). |
| 101 | [Základní informace o nepřerušený s databáze SQL Azure](sql-database-business-continuity.md) | Zjistěte, jak databáze SQL Azure podporuje nepřerušený provoz cloudu a obnovení databáze a pomáhá chránit důležitých cloudu spuštěné. |
| 102 | [Jak obnovit jednu tabulku ze zálohy databáze SQL Azure](sql-database-cloud-migrate-restore-single-table-azure-backup.md) | Zjistěte, jak obnovit jednu tabulku ze zálohy databáze SQL Azure. |
| 103 | [Návrh žádost o obnovení havárie cloudu pomocí aktivní Geo replikace databáze SQL](sql-database-designing-cloud-solutions-for-disaster-recovery.md) | Zjistěte, jak navrhnout cloudového havárie obnovení řešení pro obchodní kontinuitu plánování pomocí geo replikace pro zálohování dat aplikace s databáze SQL Azure. |
| 104 | [Obnovení databáze SQL Azure nebo převzetí sekundární](sql-database-disaster-recovery.md) | Informace o obnovení databáze ze regionálního datacentra výpadku nebo selhání Azure SQL aktivní Geo-replikace databáze a obnovení Geo schopnosti. |
| 105 | [Provádění havárie obnovení podrobností](sql-database-disaster-recovery-drills.md) | Přečtěte si pokyny a osvědčené postupy pro používání databáze SQL Azure provádět cvičení obnovení havárie, které vám pomohou udržovat vaše velice důležité podnikových aplikací pružné selhání a výpadků. |
| 106 | [Strategie obnovení havárie aplikací pomocí fondu pružná databáze SQL](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md) | Zjistěte, jak navrhnout cloudové řešení pro obnovení havárie volbou vpravo převzetí vzorku. |
| 107 | [Použití třídy RecoveryManager pro řešení potíží s mapou shard](sql-database-elastic-database-recovery-manager.md) | Použití třídy RecoveryManager k řešení problémů s mapováním shard |
| 108 | [Import souboru BACPAC vytvoření databáze Azure SQL](sql-database-import.md) | Vytvoření databáze Azure SQL importováním existujícího souboru BACPAC. |
| 109 | [Obnovení databáze SQL Azure předchozí bod pomocí portálu Azure](sql-database-point-in-time-restore-portal.md) | Obnovení databáze SQL Azure předchozí bodu. |
| 110 | [Obnovení databáze SQL Azure předchozího bodu pomocí prostředí PowerShell](sql-database-point-in-time-restore-powershell.md) | Obnovení databáze SQL Azure předchozí bodu |
| 112 | [Obnovit databázi Azure SQL pomocí zálohy automatické databáze](sql-database-recovery-using-backups.md) | Informace o obnovení v okamžiku, který umožňuje vrátit databáze SQL Azure k předchozí bodu v čase (až 35 dní). |
| 113 | [Obnovení odstraněné databáze Azure SQL pomocí portálu Azure](sql-database-restore-deleted-database-portal.md) | Obnovení odstraněné databáze Azure SQL (Azure portál). |
| 114 | [Obnovení odstraněné databáze SQL Azure pomocí prostředí PowerShell](sql-database-restore-deleted-database-powershell.md) | Obnovení odstraněné databáze SQL Azure (Powershellu). |
| 115 | [Obnovení databáze předchozí bod, obnovit odstraněnou databázi nebo obnovení z výpadku Centrum dat](sql-database-troubleshoot-backup-and-restore.md) | Zjistěte, jak obnovit databázi cloudu chyby a výpadků pomocí zálohování a repliky v databázi SQL Azure. |



## <a name="migrate"></a>Migrace

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 116 | [Export do BACPAC soubor pomocí SqlPackage databáze SQL serveru](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md) | Databázi Microsoft Azure SQL, migrace databáze export databáze, export do souboru BACPAC, sqlpackage |
| 117 | [Export databáze SQL serveru do souboru BACPAC pomocí SQL Server Management Studio](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) | Databázi Microsoft Azure SQL, migrace databáze export databáze, export do souboru BACPAC, Průvodce exportem aplikace osy dat |
| 118 | [Import k SQL databázi ze souboru BACPAC pomocí SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md) | Import databáze databázi Microsoft Azure SQL, migrace databáze, importovat BACPAC, sqlpackage |
| 119 | [Import z BACPAC k SQL databázi pomocí SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) | Databázi Microsoft Azure SQL databáze nasadit, migrace databáze, import z databáze, export databáze, Průvodce přenesením |
| 120 | [Migrace databáze SQL serveru k SQL databázi pomocí nasazení databázi Průvodce databáze Microsoft Azure](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md) | Databázi Microsoft Azure SQL, migrace databáze, Průvodce databáze Microsoft Azure |
| 121 | [Migrace databáze SQL serveru k databázi SQL Azure pomocí transakční replikace](sql-database-cloud-migrate-compatible-using-transactional-replication.md) | Import databáze, databázi Microsoft Azure SQL, migrace databáze transakční replikace |
| 122 | [Určení kompatibility databáze SQL pomocí SqlPackage.exe](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md) | Databázi Microsoft Azure SQL, migrace databáze, databáze SQL kompatibilita, SqlPackage |
| 123 | [Umožňuje určit databáze SQL kompatibility před migrací k databázi SQL Azure SQL Server Management Studio](sql-database-cloud-migrate-determine-compatibility-ssms.md) | Databázi Microsoft Azure SQL, migrace databáze, databáze SQL kompatibilita dat osy aplikace exportem |
| 124 | [Pomocí Průvodce přenesením SQL Azure řešení Server SQL databáze problémy s kompatibilitou před migrací k databázi SQL Azure](sql-database-cloud-migrate-fix-compatibility-issues.md) | Databázi Microsoft Azure SQL, migrace databáze, kompatibilita, Průvodce přenesením SQL Azure |
| 125 | [Migrovat databázi SQL serveru k databázi Azure SQL pomocí služby SQL Server Data Tools for Visual Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md) | Databázi Microsoft Azure SQL, migrace databáze, kompatibilita, SQL Azure Průvodce přenesením SSDT |
| 126 | [Řešení problémů kompatibility databáze SQL serveru pomocí SQL Server Management Studio před migrací k SQL databázi](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md) | Databázi Microsoft Azure SQL, migrace databáze, kompatibilita, Průvodce přenesením SQL Azure |
| 127 | [Archivace databáze Azure SQL do souboru BACPAC pomocí prostředí PowerShell](sql-database-export-powershell.md) | Archivace databáze Azure SQL do souboru BACPAC pomocí prostředí PowerShell |
| 128 | [Import souboru BACPAC vytvoření databáze Azure SQL pomocí prostředí PowerShell](sql-database-import-powershell.md) | Import souboru BACPAC vytvoření databáze Azure SQL pomocí prostředí PowerShell |
| 129 | [Načtení dat ze souboru CSV do Azure SQL datový sklad (textových souborů)](sql-database-load-from-csv-with-bcp.md) | Velikost malé dat používá bcp k importu dat do databáze SQL Azure. |
| 130 | [Přesuňte databází mezi servery, mezi předplatná a a odhlášení z Azure](sql-database-troubleshoot-moving-data.md) | Rychlé kroky chcete kopírovat, přesunout a migraci dat a databází v databázi SQL Azure. |



## <a name="move-data-in-and-out"></a>Přesunutí dat nebo zmenšit

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 131 | [Migrace databáze SQL serveru k SQL databázi v cloudu](sql-database-cloud-migrate.md) | Zjistěte, Co takhle místního serveru SQL Server migrace databáze k databázi SQL Azure v cloudu. Otestovat funkce pro kompatibilitu před migrace databáze pomocí nástroje pro migraci z databáze. |
| 132 | [Zkopírujte databázi Azure SQL](sql-database-copy.md) | Vytvořit kopii databáze Azure SQL |
| 133 | [Archivace databáze Azure SQL do souboru BACPAC pomocí portálu Azure](sql-database-export.md) | Archivace databáze Azure SQL do souboru BACPAC pomocí portálu Azure |



## <a name="security"></a>Zabezpečení

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 134 | [Připojení k databázi SQL nebo SQL datový sklad pomocí ověřování služby Azure Active Directory](sql-database-aad-authentication.md) | Zjistěte, jak připojit k databázi SQL pomocí ověřování služby Azure Active Directory. |
| 135 | [Vždy zašifrovaných: Ochrana citlivá data do SQL databáze a uložte šifrovacího klíče úložiště certifikátů Windows](sql-database-always-encrypted.md) | Chrání citlivá data v databázi SQL v minutách. |
| 136 | [Vždy zašifrovaných: Ochrana citlivá data v databázi SQL a ukládat šifrovacího klíče trezoru klíč Azure](sql-database-always-encrypted-azure-key-vault.md) | Chrání citlivá data v databázi SQL v minutách. |
| 137 | [Databáze SQL – podpora klientů a změny IP koncový bod pro auditování](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md) | Informace o databáze SQL podpora klientů a IP změny koncový bod pro auditování. |
| 138 | [Konfigurace pravidel brány firewall úrovni serveru databáze SQL Azure pomocí prostředí PowerShell](sql-database-configure-firewall-settings-powershell.md) | Informace o konfiguraci brány firewall pro IP adresy, které přístup k databázím Azure SQL. |
| 139 | [Konfigurace pravidel brány firewall úrovni serveru databáze SQL Azure pomocí rozhraní REST API](sql-database-configure-firewall-settings-rest.md) | Informace o konfiguraci brány firewall pro IP adresy, které přístup k databázím Azure SQL. |
| 140 | [Konfigurace pravidel serveru a databáze úrovně brány firewall databáze SQL Azure pomocí T-SQL](sql-database-configure-firewall-settings-tsql.md) | Informace o konfiguraci brány firewall pro IP adresy, které přístup k databázím Azure SQL. |
| 141 | [Začínáme s SQL databáze dynamických dat maskování (Azure portál)](sql-database-dynamic-data-masking-get-started.md) | Jak začít s SQL databáze dynamických dat maskování na portálu Azure |
| 142 | [Začínáme s SQL databáze dynamických dat maskování (klasické portál Azure)](sql-database-dynamic-data-masking-get-started-portal.md) | Jak začít s SQL databáze dynamických dat maskování klasické portálu Azure |
| 143 | [Její konfiguraci databáze SQL Azure \- základní informace](sql-database-firewall-configure.md) | Zjistěte, jak nakonfigurovat bránu firewall databáze SQL s pravidly server a databázi úrovně bránu firewall a spravovat přístup. |
| 144 | [Databáze SQL kurz: vytvoření databáze SQL uživatelských účtů k přístupu a spravovat databáze](sql-database-get-started-security.md) | Naučte se vytvořit uživatelské účty a spravovat databáze. |
| 145 | [Databáze SQL a tak mohli ověřovat: udělení přístupu](sql-database-manage-logins.md) | Další informace o správě zabezpečení databáze SQL konkrétně způsob správy zabezpečení databáze access a přihlaste pomocí účtu hlavní úrovni serveru. |
| 146 | [Pokyny k používání zabezpečení Azure SQL databáze a omezení](sql-database-security-guidelines.md) | Informace o databázi Microsoft Azure SQL pokyny a omezení týkající se zabezpečení. |
| 147 | [Začínáme s zjišťování hrozbou, že databáze SQL](sql-database-threat-detection-get-started.md) | Jak začít s hrozbou, že zjišťování databáze SQL Azure portálu |
| 148 | [Jak provádět běžné úkoly správy například resetování hesla správce v databázi SQL Azure](sql-database-troubleshoot-permissions.md) | Popisuje, jak provádět běžné úkoly správy v databázi SQL. Například resetování hesla správce, poskytnutí a odebrání přístup. |



## <a name="manage-your-database"></a>Správa databáze aplikace

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 149 | [Začínáme s auditování databáze SQL](sql-database-auditing-get-started.md) | Začínáme s auditování databáze SQL |
| 150 | [Konfigurace brány firewall na úrovni serveru pravidlo databáze SQL Azure pomocí portálu Azure](sql-database-configure-firewall-settings.md) | Informace o konfiguraci brány firewall pro IP adresy, které mají přístup Azure SQL serveru. |
| 151 | [Zkopírujte databázi SQL Azure pomocí portálu Azure](sql-database-copy-portal.md) | Vytvořit kopii databáze Azure SQL |
| 152 | [Správa databáze SQL Azure pomocí automatizace Azure](sql-database-manage-automation.md) | Informace o použití služby Azure automatizaci ke správě databáze Azure SQL ve velkém měřítku. |
| 153 | [Přehled: nástroje pro správu databáze SQL](sql-database-manage-overview.md) | Srovnání nástroje a možnosti pro správu databáze SQL Azure |
| 154 | [Sledování databáze SQL Azure pomocí Správa dynamických zobrazení](sql-database-monitoring-with-dmvs.md) | Informace o zjišťování a Diagnostika běžné problémy s výkonem pomocí Správa dynamických zobrazení sledování databázi Microsoft Azure SQL. |
| 155 | [Zabezpečení databáze SQL](sql-database-security.md) | Další informace o zabezpečení databáze SQL Azure a SQL Server, včetně rozdíly mezi cloudu a SQL Server místní až přijde ověření se tak mohli ověřovat, zabezpečení připojení, šifrování a dodržování předpisů. |



## <a name="extended-events"></a>Rozšířené události

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 156 | [Kód cílové soubor události rozšířené událostí v databázi SQL](sql-database-xevent-code-event-file.md) | Příklad dvoufázový kód, který ukazuje cíl události souborů v rozšířené událost na databázi SQL Azure poskytuje prostředí PowerShell a jazyce Transact-SQL. Azure úložiště je povinná část tento scénář. |
| 157 | [Vyzvání kódu cílové vyrovnávací paměť rozšířené událostí v databázi SQL](sql-database-xevent-code-ring-buffer.md) | Obsahuje ukázky kód jazyce Transact-SQL, která je nastavená jako snadno a rychle pomocí vyrovnávací cílové databáze SQL Azure. |
| 158 | [Rozšířené událostí v databázi SQL](sql-database-xevent-db-diff-from-svr.md) | Popisuje v databázi SQL Azure rozšířené události (XEvents) a jak relace událostí trochu lišit relace událostí v Microsoft SQL Server. |



## <a name="geo-replication"></a>Replikace GEO

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 159 | [Zahájení přepojení plánované nebo neplánované databáze SQL Azure pomocí portálu Azure](sql-database-geo-replication-failover-portal.md) | Zahájení konverzace přepojení plánované nebo neplánované databáze SQL Azure pomocí portálu Azure |
| 160 | [Zahájení konverzace přepojení plánované nebo neplánované pro databáze SQL Azure pomocí prostředí PowerShell](sql-database-geo-replication-failover-powershell.md) | Zahájení konverzace přepojení plánované nebo neplánované databáze SQL Azure pomocí prostředí PowerShell |
| 161 | [Zahájení konverzace přepojení plánované nebo neplánované pro databáze SQL Azure pomocí jazyce Transact-SQL](sql-database-geo-replication-failover-transact-sql.md) | Zahájení konverzace přepojení plánované nebo neplánované databáze SQL Azure pomocí jazyce Transact-SQL |
| 162 | [Přehled: SQL aktivní Geo replikace databáze](sql-database-geo-replication-overview.md) | Aktivní Geo replikace umožňuje nastavit 4 kopie databáze v některém z Azure datacentrech. |
| 163 | [Konfigurace Geo replikace databáze SQL Azure pomocí portálu Azure](sql-database-geo-replication-portal.md) | Konfigurace Geo replikace databáze SQL Azure pomocí portálu Azure |
| 164 | [Konfigurace Geo replikace databáze Azure SQL pomocí prostředí PowerShell](sql-database-geo-replication-powershell.md) | Konfigurace aktivní Geo replikace databáze SQL Azure pomocí prostředí PowerShell |
| 165 | [Správa zabezpečení databáze SQL Azure po obnovení havárie](sql-database-geo-replication-security-config.md) | Toto téma vysvětluje otázky bezpečnosti pro správu zabezpečení po obnovení databáze nebo selhání. |
| 166 | [Konfigurace Geo replikace databáze Azure SQL pomocí Transact-SQL](sql-database-geo-replication-transact-sql.md) | Konfigurace Geo replikace databáze SQL Azure pomocí jazyce Transact-SQL |
| 167 | [GEO obnovení databáze SQL Azure ze zálohy geo nadbytečné pomocí portálu Azure](sql-database-geo-restore-portal.md) | Obnovení GEO databáze SQL Azure ze zálohy geo nadbytečné (Azure portál). |
| 168 | [Obnovení databáze SQL Azure ze zálohy geo nadbytečné pomocí prostředí PowerShell](sql-database-geo-restore-powershell.md) | Obnovení databáze SQL Azure do nového serveru ze geo nadbytečné zálohy |



## <a name="upgrade"></a>Upgrade

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 169 | [Vylepšení výkonu dotazu s kompatibilitou 130 úroveň v databázi SQL Azure](sql-database-compatibility-level-query-performance-130.md) | Kroky a nástroje pro určení, které úroveň kompatibility je nejvhodnější pro databáze aplikace na databázi SQL Azure nebo Microsoft SQL Server |
| 170 | [Správa postupné inovace aplikací cloudu pomocí SQL aktivní Geo-replikace databáze](sql-database-manage-application-rolling-upgrade.md) | Naučte se používat geo replikace databáze SQL Azure podporuje online upgrady aplikace cloudu. |
| 171 | [Upgrade na V12 databáze SQL Azure pomocí portálu Azure](sql-database-upgrade-server-portal.md) | Vysvětluje, jak upgradovat na V12 databáze SQL Azure včetně upgrade webu a Business databází a upgrade serveru V11 migrace své databáze přímo do fondu pružná databáze pomocí portálu Azure. |
| 172 | [Upgrade na V12 databáze SQL Azure pomocí prostředí PowerShell](sql-database-upgrade-server-powershell.md) | Vysvětluje, jak upgradovat na V12 databáze SQL Azure včetně upgrade webu a Business databází a upgrade serveru V11 migrace své databáze přímo do fondu pružná databáze pomocí Powershellu. |
| 173 | [Plánování a příprava na upgrade na SQL databáze V12](sql-database-v12-plan-prepare-upgrade.md) | Popisuje přípravy a omezení zahrnuté do upgradu na verzi V12 databáze SQL Azure. |
| 174 | [Co je nového v SQL databázi V12](sql-database-v12-whats-new.md) | Popisuje, proč bude využívat upgradu na verzi V12 systémy firmy, které používají databáze SQL Azure v cloudu. |
| 175 | [Web a Business Edition Jahoda – nejčastější dotazy](sql-database-web-business-sunset-faq.md) | Zjistíte, kdy databáze Azure SQL Web a Business se už nepoužívá a seznamte se s funkcí a možností nové úrovně služby. |



## <a name="tools"></a>Nástroje

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 176 | [Správa databáze Azure SQL pomocí prostředí PowerShell](sql-database-command-line-tools.md) | Azure SQL databáze Správa prostřednictvím Powershellu. |
| 177 | [Databáze SQL kurz: připojení k databázi Azure SQL aplikace Excel a vytvářet sestavy](sql-database-connect-excel.md) | Zjistěte, jak se připojit k databázi Azure SQL v cloudu aplikaci Microsoft Excel. Import dat do Excelu pro vytváření sestav a zkoumání dat. |
| 178 | [Vytvoření databáze SQL a provádění běžných úloh nastavení databáze pomocí rutin prostředí PowerShell](sql-database-get-started-powershell.md) | Podívejte se na vytvoření databáze SQL pomocí prostředí PowerShell. Běžné úkoly instalace databáze můžete spravovat pomocí rutin prostředí PowerShell. |
| 179 | [Začínáme se synchronizací dat Azure SQL (verze Preview)](sql-database-get-started-sql-data-sync.md) | Tento kurz vám pomůže začít s Azure SQL Data Sync (verze Preview). |
| 180 | [Správa databáze SQL Azure pomocí SQL Server Management Studio](sql-database-manage-azure-ssms.md) | Naučte se používat SQL Server Management Studio ke správě databáze SQL servery a databází. |
| 181 | [Správa databáze SQL Azure pomocí portálu Azure](sql-database-manage-portal.md) | Naučte se používat portál Azure ke správě relační databáze v cloudu pomocí portálu Azure. |
| 182 | [Změna služby osy a výkonu úrovně (cena za osy) databáze SQL pomocí prostředí PowerShell](sql-database-scale-up-powershell.md) | Změna vrstvy služeb a úroveň výkonu databáze Azure SQL ukazuje, jak změnit velikost databáze SQL nahoru nebo dolů v prostředí PowerShell. Změna ceny osy databáze Azure SQL pomocí prostředí PowerShell. |
| 183 | [Použití SQL Server Management Studio v RemoteApp připojení k databázi SQL Azure](sql-database-ssms-remoteapp.md) | Pomocí tohoto kurzu se dozvíte, jak můžete SQL Server Management Studio v Azure RemoteApp u zabezpečení a výkonu při připojení k databázi SQL |



## <a name="reference"></a>Odkaz

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 184 | [Knihovny připojení databáze SQL a SQL Server](sql-database-libraries.md) | Seznam minimální číslo verze pro každý ovladač využívající klientských programů pro připojení k databázi SQL Azure nebo na Microsoft SQL Server. Verze informace o ovladače vydané komunity neinstaluje z Microsoft je k dispozici odkaz. |
| 185 | [Omezení zdrojů Azure SQL databáze](sql-database-resource-limits.md) | Tato stránka popisuje některé běžné omezení prostředků pro databázi SQL Azure. |
| 186 | [Rozdíly SQL databáze Transact-SQL Azure](sql-database-transact-sql-information.md) | Skalární funkce příkazy, které jsou menší než plně podporované v databázi SQL Azure |



## <a name="miscellaneous"></a>Různé

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 187 | [Zkopírujte databázi Azure SQL pomocí prostředí PowerShell](sql-database-copy-powershell.md) | Vytvoření kopie databáze Azure SQL pomocí prostředí PowerShell |
| 188 | [Zkopírujte databázi Azure SQL pomocí jazyce Transact-SQL](sql-database-copy-transact-sql.md) | Vytvoření kopie databáze Azure SQL pomocí jazyce Transact-SQL |
| 189 | [Omezení obecné databáze Azure SQL a pokyny](sql-database-general-limitations.md) | Tato stránka popisuje některé obecné omezení pro databázi SQL Azure, jakož i oblasti spolupráce a podporu. |
| 190 | [Databáze SQL ceny osy doporučení](sql-database-service-tier-advisor.md) | Při změně ceny úrovní Azure portálu ceny osy doporučení jsou za předpokladu, že doporučená osy, který je nejlepší vhodný pro spuštění pracovního vytížení existující Azure SQL databáze společnosti. Ceny úrovní popisují služby osy a výkonu úrovně databázi SQL. |


&nbsp;

<hr/>

<a name="extras_append"></a>

## <a name="extras"></a>Doplňky

<a name="extra_related_articles"></a>

### <a name="related-articles-from-other-azure-services"></a>Související články z jiných služeb, Azure

- [Obecnějším údajům aplikace služby Azure aplikace v Azure](../app-service-web/web-sites-backup.md)

<a name="extra_documentation_tools"></a>

### <a name="documentation-tools"></a>Přečtěte následující dokumentaci pro nástroje

- [Aktualizace se ohlásí, pro databáze SQL Azure](http://azure.microsoft.com/updates/?service=sql-database)

- [Vyhledávání všech typů si přečtěte následující dokumentaci Microsoft Azure pomocí filtrů](http://azure.microsoft.com/search/documentation/)

<a name="extra_learning_path_graphics"></a>

### <a name="learning-path-graphics"></a>Přehled výukových cestu grafiky

- [Přehled výukových grafiky cesty pro všechny služby Microsoft Azure](http://azure.microsoft.com/documentation/learning-paths/)

- [Naučná stezka: Další databáze Azure SQL](http://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database/)

- [Naučná stezka: Pružná měřítko databáze SQL](http://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/)


<!--
AzIxAA.ExtraAppend.sql-database.txt
C:\_MainW\VS15\AzureIndexAllArticles\AzureIndexAllArticles\In-Out\In\

This bullet link is improperly disallowed by publishing automation due to presence of string 'docuXXmentation/arXXticles':
- [Search SQL Database documentation, with filters](http://azure.microsoft.com/docuXXmentation/arXXticles/?service=sql-database)
-->


