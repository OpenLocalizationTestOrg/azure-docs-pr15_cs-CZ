<properties 
    pageTitle="Databáze DocumentDB otázky – nejčastější dotazy | Microsoft Azure" 
    description="Projděte si odpovědi na nejčastější dotazy týkající se Azure DocumentDB NoSQL dokumentu databáze služby JSON. Odpovědi na otázky databáze ohledně kapacita úrovně výkonu a měřítka." 
    keywords="Databáze, často kladené otázky documentdb, azure, Microsoft azure otázky"
    services="documentdb" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="mimig"/>


#<a name="frequently-asked-questions-about-documentdb"></a>Nejčastější dotazy k DocumentDB

## <a name="database-questions-about-microsoft-azure-documentdb-fundamentals"></a>Databáze dotazy týkající se základy Microsoft Azure DocumentDB

### <a name="what-is-microsoft-azure-documentdb"></a>Co je Microsoft Azure DocumentDB? 
Microsoft Azure DocumentDB je svěží rychlé a planet měřítko NoSQL dokumentu databáze jako služba, která nabízí dotazů na uvolnit schématu data, pomáhají zajistit, která dokáže nahradit a spolehlivé výkonu a umožňuje rychlý vývoj prostřednictvím spravovaných platformy podporovaným power a dosah Microsoft Azure. DocumentDB je správné řešení pro web, mobilní, hraní a IoT aplikací při předvídatelná výkon, dostupnost, zhoršeným latence a uvolnit schématu datovém modelu jsou základní požadavky. DocumentDB poskytuje flexibilitu schématu a rozsáhle indexování pomocí nativního JSON datový model a obsahuje transakční podporu více dokumentů pomocí integrovaného JavaScript.  
  
Další otázky databáze, odpovědi a pokyny k nasazení a pomocí této služby najdete v článku [DocumentDB si přečtěte následující dokumentaci stránky](https://azure.microsoft.com/documentation/services/documentdb/).

### <a name="what-kind-of-database-is-documentdb"></a>Jaký druh databáze je DocumentDB?
DocumentDB je NoSQL dokumentu orientací databázi, která jsou uložená data ve formátu JSON.  DocumentDB podporuje vnořené, ztlumí contained datové struktury, které můžete vyhledávat pomocí bohaté DocumentDB [gramatiky dotaz SQL](documentdb-sql-query.md). DocumentDB poskytuje vysoký výkon transakční zpracování na straně serveru JavaScript prostřednictvím [uložené procedury, aktivačních událostí a uživatelsky definované funkce](documentdb-programming.md). Databáze taky podporuje vývojář optimalizovat konzistence úrovně s přidruženými [úrovně výkonu](documentdb-performance-levels.md).
 
### <a name="do-documentdb-databases-have-tables-like-a-relational-database-rdbms"></a>Mají databází DocumentDB tabulkami jako relační databáze (RDBMS)?
Ne, DocumentDB ukládá datové sady dokumentů JSON.  Informace o zdrojích DocumentDB najdete v tématu [DocumentDB zdroje modelu a koncepty](documentdb-resources.md). Další informace o způsobu řešení NoSQL například DocumentDB liší od relační řešení najdete v tématu [NoSQL a SQL](documentdb-nosql-vs-sql.md).

### <a name="do-documentdb-databases-support-schema-free-data"></a>Podporují DocumentDB databází schématu bez dat?
Ano, DocumentDB umožňuje aplikacím ukládat libovolného JSON dokumenty bez definice schématu a tipy. Data lze okamžitě dotazu prostřednictvím rozhraní DocumentDB SQL dotazu.   

### <a name="does-documentdb-support-acid-transactions"></a>Podporuje DocumentDB kyseliny transakce?
Ano, DocumentDB podporuje více dokumentů transakce vyjádřený JavaScript uložené procedury a aktivace. Obor vymezený na jeden oddíl v rámci každé kolekce a spustit kyseliny sémantiku jako všechny transakce nebo nic samostatný z jiných souběžně provádění kód a uživatel žádosti o.  Pokud jsou výjimky vyvolání uskutečňování straně serveru kódu jazyka JavaScript aplikace, je celý transakce vrátit zpět. Další informace o transakce najdete v článku [databázové transakce program](documentdb-programming.md#database-program-transactions).

### <a name="what-are-the-typical-use-cases-for-documentdb"></a>Co jsou to typické použití balení DocumentDB?  
DocumentDB je vhodný pro nový web, mobilním, hraní a aplikací IoT kde automatické měřítko, předvídatelná výkonu rychlé pořadí doby odezvy milisekund a možnost dotazu myši uvolněte schématu dat je důležité. DocumentDB různě rychlý vývoj a podporu nepřetržitý iterace aplikace datových modelů. Aplikace, které spravují uživatele generovaného obsah a data jsou [běžné případy použití pro DocumentDB](documentdb-use-cases.md).  

### <a name="how-does-documentdb-offer-predictable-performance"></a>Jak DocumentDB nabízejí předvídatelná výkon?
[Žádost o jednotku](documentdb-request-units.md) je míra výkon v DocumentDB. 1 RU odpovídá výkonu GET dokumentu 1KB. Všechny operace v DocumentDB včetně čtení, zápisy, dotazech SQL a spuštění uložené procedury má deterministický RU hodnoty založené na výkon potřebná k dokončení operace. Místo přemýšlet o využití procesoru, vstupů/výstupů paměti a jaký každá má vliv na výkon aplikace, si můžete představit z hlediska jednu míru RU.

Jednotlivé kolekce DocumentDB lze rezervovat s zřizování výkon z hlediska RUs výkon sekundu. U aplikací všechny měřítko můžete testu výkonnosti jednotlivé požadavky na změřit jejich RU hodnoty a poskytování kolekcí zpracovávání celkový součet žádost o jednotky přes všechny žádosti o. Můžete taky škálování nebo Neomezovat vaší kolekci výkon jako potřeb evolve aplikace. Další informace o žádosti o jednotky a nápovědu zjištění kolekci potřebuje, přečtěte [Správa výkonu a kapacity](documentdb-manage.md) a zkuste [kalkulačky výkon](https://www.documentdb.com/capacityplanner). 

### <a name="is-documentdb-hipaa-compliant"></a>Je kompatibilní se standardem DocumentDB HIPAA?
Ano, DocumentDB je HIPAA kompatibilní. HIPAA naváže požadavky pro použití, oznámení a ochrana informací jednotlivě identifikovatelné stavu. Další informace najdete v tématu [Centrum zabezpečení aplikace Microsoft](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA).

### <a name="what-are-the-storage-limits-of-documentdb"></a>Jaká jsou omezení úložiště DocumentDB? 
Existuje neomezeno teoretický celkovém množství dat, která mohou být uloženy kolekce v DocumentDB. Pokud chcete uložit víc než 250 GB dat v rámci jedné kolekce, ujistěte se, [Kontaktujte podporu](documentdb-increase-limits.md) mít účet kvótu lepší. 

### <a name="what-are-the-throughput-limits-of-documentdb"></a>Jaká jsou omezení výkon DocumentDB? 
Existuje neomezeno teoretický celkovém množství výkon podporující kolekce v DocumentDB, pokud váš pracovní zátěž může být rovnoměrně zhruba mezi dostatečně velký počet oddílů klíče. Pokud chcete být vyšší než 250 000 žádost jednotky/druhé za kolekce nebo účet, ujistěte se, [Kontaktujte podporu](documentdb-increase-limits.md) mít účet kvótu lepší. 

### <a name="how-much-does-microsoft-azure-documentdb-cost"></a>Kolik stojí Microsoft Azure DocumentDB?
Získáte na stránce [Podrobnosti o cenách DocumentDB](https://azure.microsoft.com/pricing/details/documentdb/) podrobnosti. DocumentDB nadbytečným poplatkům jsou určena počtu kolekcí používá, počet hodin kolekce jste online a spotřebované úložiště a zřizování výkon pro každou kolekci. 

### <a name="is-there-a-free-account-available"></a>Je bezplatný účet k dispozici?
Pokud začínáte Azure, můžete registraci [Azure bezplatný účet](https://azure.microsoft.com/free/), který vám 30 dní a 200 $ zkusit Azure služby. Nebo pokud máte předplatné aplikace Visual Studio, máte nárok na [150 v bezplatné Azure přeplatky měsíčně](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) na Azure služby používat.  

### <a name="how-can-i-get-additional-help-with-documentdb"></a>Jak můžu získat další nápovědu k DocumentDB?
Pokud potřebujete veškerá Nápověda, kontaktujte nás na [Přetečení zásobníku](http://stackoverflow.com/questions/tagged/azure-documentdb) [Azure DocumentDB MSDN vývojář fóra](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureDocumentDB)nebo naplánování [1:1 konverzace se na tým technické DocumentDB](http://www.askdocdb.com/). Dodržovat předpisy neustálý přehled o nejnovější informace DocumentDB a funkce, postupujte podle nám na [Twitter](https://twitter.com/DocumentDB).

## <a name="set-up-microsoft-azure-documentdb"></a>Nastavení Microsoft Azure DocumentDB

### <a name="how-do-i-sign-up-for-microsoft-azure-documentdb"></a>Jak můžu registrace pro Microsoft Azure DocumentDB?
Microsoft Azure DocumentDB je dostupný [Portál Azure][azure-portal].  Nejdřív musíte se přihlásit k odběru Microsoft Azure.  Po registraci Microsoft Azure předplatné, můžete přidat účet DocumentDB k předplatnému Azure. Pokyny k přidání účtu DocumentDB naleznete v tématu [Vytvoření účet DocumentDB databáze](documentdb-create-account.md).   

### <a name="what-is-a-master-key"></a>Co je předloha klíč?
Hlavní klíč je tokenu zabezpečení pro přístup k všechny zdroje pro účet. Osobám s klávesou četl a oprávnění k zápisu do všech zdrojů v účet databázi. Provádějte opatrně distribuce hlavní klíče. Primární klíč hlavní a vedlejší hlavní klíč jsou k dispozici v **klíče **zásuvné [Azure portál][azure-portal]. Podrobnosti o klíčů najdete v článku [zobrazení, kopírovat a regenerovat přístupových kláves z verze](documentdb-manage-account.md#keys).

### <a name="how-do-i-create-a-database"></a>Jak vytvořit databázi?
Můžete vytvořit databáze pomocí [Portálu Azure]() podle popisu v tématu [vytvoření databáze DocumentDB](documentdb-create-database.md), jednu [DocumentDB SDK](documentdb-sdk-dotnet.md)nebo prostřednictvím rozhraní [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx).  

### <a name="what-is-a-collection"></a>Co je kolekce?
Kolekce je kontejner JSON dokumentů a přidružené aplikace logiku JavaScript. Kolekce je fakturaci entitu, kde [náklady](documentdb-performance-levels.md) , je určený podle výkon a storaged použít. Kolekce může zahrnovat jeden nebo více oddílů/servery a lze přizpůsobit zpracovat prakticky neomezené množství úložiště nebo výkon.

Kolekce jsou také fakturační entity pro DocumentDB. Jednotlivé kolekce je fakturovány hodinové na základě zřizování výkon a úložiště používá. Další informace najdete v tématu [DocumentDB ceny](https://azure.microsoft.com/pricing/details/documentdb/).  

### <a name="how-do-i-set-up-users-and-permissions"></a>Nastavení uživatelů a oprávnění
Můžete vytvořit uživatelé a oprávnění pomocí jednoho [DocumentDB SDK](documentdb-sdk-dotnet.md) nebo prostřednictvím rozhraní [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx).   

## <a name="database-questions-about-developing-against-microsoft-azure-documentdb"></a>Databáze dotazy týkající se vývoj proti Microsoft Azure DocumentDB

### <a name="how-to-do-i-start-developing-against-documentdb"></a>Jak udělat spouštění vývoj proti DocumentDB?
[SDK](documentdb-sdk-dotnet.md) jsou k dispozici pro .NET Python, Node.js, JavaScript a Java.  Vývojáři můžete taky využít [RESTful rozhraní API HTTP](https://msdn.microsoft.com/library/azure/dn781481.aspx) chcete provést interakci s DocumentDB zdrojů z různých platformy a jazyky. 

Ukázky DocumentDB [.NET](documentdb-dotnet-samples.md), [Java](https://github.com/Azure/azure-documentdb-java), [Node.js](documentdb-nodejs-samples.md)a [Python](documentdb-python-samples.md) SDK jsou dostupné na GitHub.

### <a name="does-documentdb-support-sql"></a>Podporuje DocumentDB SQL?
Jazyka DocumentDB SQL dotazu je lepší podmnožiny dotazu funkcí podporovaných SQL. Dotazovací jazyk DocumentDB SQL nabízí hierarchické RTF "a" relační operátory a rozšíření prostřednictvím uživatele na základě JavaScript definované funkce (UDF). JSON gramatiky umožňuje modelování JSON dokumentů jako stromy s popisky jako uzly stromu používaný automatické indexování postupů DocumentDB jak dialektu dotaz SQL DocumentDB.  Pro informace o používání gramatiky SQL najdete v tématu [Dotazu DocumentDB] [ query] článek.

### <a name="what-are-the-data-types-supported-by-documentdb"></a>Jaké jsou datových typů podporovaných DocumentDB?
Základní datové typy podporované v DocumentDB jsou stejné jako JSON. JSON má jednoduchý typ systému, který se skládá z řetězce, čísla (IEEE754 dvojitou přesností) a logické hodnoty – PRAVDA, NEPRAVDA a Null.  Složitější datových typů jako data a času, Guid, Int64 a geometrie představovat ve formátu JSON DocumentDB i vytvářením vnořené objektů pomocí operátoru {} a maticemi použití operátoru []. 

### <a name="how-does-documentdb-provide-concurrency"></a>Jak DocumentDB poskytnout souběžné?
DocumentDB podporuje řízení optimistické souběžné (OCC) prostřednictvím protokolu HTTP entity značky nebo etags. Každý zdroj DocumentDB má etag a etag je nastavený na serveru při každé aktualizaci dokumentu. Záhlaví etag a aktuální hodnotu jsou součástí všechny odpovědi na zprávy. Etags lze záhlaví If POZVYHLEDAT umožňují serveru se rozhodnout, pokud mají být aktualizovány zdroje. Když POZVYHLEDAT hodnota je hodnota etag kontrolovaná proti. Pokud hodnota etag odpovídá hodnota etag serveru, bude aktualizován zdroje. Pokud již není aktuální etag, server odmítne operace s "HTTP 412 předpokladem chyba" kód odpovědi. Klient pak muset znovu načíst příslušný zdroj k získání aktuální hodnota etag zdroje. Kromě toho etags lze záhlaví If-žádné časově POZVYHLEDAT a zjistit, pokud je potřeba opětovné načtení zdroje. 

Optimistická souběžné v .NET použijete [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx) předmětu. .NET vzorku najdete v článku [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs) ve výběru DocumentManagement na github.

### <a name="how-do-i-perform-transactions-in-documentdb"></a>Jak provádět transakce v DocumentDB
DocumentDB podporuje integrované jazyk transakce prostřednictvím JavaScript uložené procedury a aktivace. Všechny operace databáze uvnitř skripty zpracují v části izolace snímek s rozsahem kolekce je-li kolekce jedním oddílem, nebo dokumenty s stejné hodnoty klíče oddílu v rámci kolekce, pokud oddíly kolekci. Snímek verze dokumentu (ETags) je pořízené na začátku transakce a potvrzeného jenom v případě, že se mu skript. Pokud JavaScript vyvolá chybu, transakce je vrátit zpět. Další informace v tématu [programování DocumentDB straně serveru](documentdb-programming.md) .

### <a name="how-can-i-bulk-insert-documents-into-documentdb"></a>Jak lze do DocumentDB hromadně vložení dokumentů? 
Hromadně vložení dokumentů do DocumentDB třemi způsoby:

- Datové nástroje pro migraci, jak je uvedeno v [dialogu Import dat do DocumentDB](documentdb-import-data.md).
- Průzkumník dokumentů na portálu Azure, jak je uvedeno v [hromadné přidání dokumentů v Průzkumníkovi dokumentu](documentdb-view-json-document-explorer.md#BulkAdd).
- Uložené procedury, podle popisu v [programovacím DocumentDB straně serveru](documentdb-programming.md).

### <a name="does-documentdb-support-resource-link-caching"></a>Podporuje DocumentDB ukládání do mezipaměti zdroje odkaz?
Ano, protože DocumentDB je RESTful služba, odkazy zdroje jsou neměnný a můžou mezipaměti. Klienti DocumentDB můžete určit "If-žádné časově POZVYHLEDAT" záhlaví pro čtení proti všechny zdroje jako dokument nebo shromažďování a jejich místní slouží ke kopírování při serverovou verzi má změnit pouze aktualizace. 

### <a name="is-a-local-instance-of-documentdb-available"></a>Neexistuje místní instanci DocumentDB?
V současné době místní instanci DocumentDB není k dispozici. Můžete sledovat stav místní emulátoru a hlasování ho na [fórum pro názory](https://feedback.azure.com/forums/263030-documentdb/suggestions/6328798-standalone-local-instance).


[azure-portal]: https://portal.azure.com
[query]: documentdb-sql-query.md
 
