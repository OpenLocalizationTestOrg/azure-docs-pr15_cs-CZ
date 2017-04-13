<properties 
    pageTitle="Přesouvání dat mezi databázemi rozšířených cloudu | Microsoft Azure" 
    description="Vysvětluje, jak pracovat s shards a přesunutí dat pomocí vlastního hostovanou službu pomocí pružná databáze rozhraní API." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="moving-data-between-scaled-out-cloud-databases"></a>Přesouvání dat mezi rozšířených cloudu databází

Pokud jste Software vývojář služby a neočekávaně aplikace projde obrovské službu, budete muset pokryly růst. Tak můžete přidat další databáze (shards). Jak distribuovat data, která chcete nové databáze bez přerušení integrity dat? Přesuňte data z omezené databáze do nové databáze pomocí **nástroje sloučení rozdělení** .  

Nástroj rozdělit sloučení běží jako Azure webové služby. Správce nebo vývojář pomocí nástroje k přesunutí shardlets (data z shard) mezi různé databáze (shards). Nástroj používá shard mapy řízení spravovat databáze metadata služby a zajistit konzistentní mapování.

![Základní informace][1]

## <a name="download"></a>Ke stažení
[Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/)


## <a name="documentation"></a>Si přečtěte následující dokumentaci
1. [Kurz nástroj pružná databáze rozdělit hromadné korespondence](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
* [Konfigurace zabezpečení rozdělit hromadné korespondence](sql-database-elastic-scale-split-merge-security-configuration.md)
* [Otázky bezpečnosti rozdělit hromadné korespondence](sql-database-elastic-scale-split-merge-security-configuration.md)
* [Správa mapování shard](sql-database-elastic-scale-shard-map-management.md)
* [Migrace existující databáze do škálování](sql-database-elastic-convert-to-use-elastic-tools.md)
* [Pružná databázové nástroje](sql-database-elastic-scale-introduction.md)
* [Pružná Glosář databázové nástroje](sql-database-elastic-scale-glossary.md)

## <a name="why-use-the-split-merge-tool"></a>Proč používat nástroj rozdělit sloučení?

**Flexibilitu**

Aplikace je třeba roztáhněte pružně mimo limit jednoho databáze databáze SQL Azure. Pokud chcete přesunout data v případě potřeby na nové databáze a ponechá integrity pomocí nástroje.

**Rozdělit a postupně se zvětšují** 

Musíte být celkovou kapacitu pro zpracování výrazný růst. K tomu, vytvořte další kapacita sharding data a distribuce přes postupně další databáze, dokud splnění potřeb kapacity. Toto je přednostní Příklad funkce "rozdělení. 

**Sloučení katalogů do zmenšit**

Kapacita potřebuje zmenšit kvůli sezónní povahy podniku. Nástroj umožňuje přizpůsobit jednotkách méně při zpomaluje business. Funkci "korespondence" ve službě pružná měřítko rozdělit hromadné korespondence zahrnuje tohoto požadavku. 

**Správa aktivní přesunutím shardlets**

S víc klientů na databázi přidělení shardlets shards může vést k problémových kapacitu míst na některé shards. Tento postup vyžaduje znovu přidělování shardlets nebo přesunutí Zaneprázdněné shardlets do nového nebo méně utilized shards. 

## <a name="concepts--key-features"></a>Koncepty a klíčové funkce

**Služby zákazníkům hostované**

Sloučení rozdělení je Doručená jako služby hostované zákazníka. Musíte nasazení a hostitelem služby v předplatném Microsoft Azure. Balíček, si můžete stáhnout z NuGet obsahuje šablony konfigurace dokončete s informacemi pro konkrétní nasazení. V tématu [kurz rozdělit sloučení](sql-database-elastic-scale-configure-deploy-split-and-merge.md) podrobnosti. Vzhledem služby Azure předplatného, můžete nastavit a konfigurace Většina aspekty zabezpečení služby. Výchozí šablonu obsahuje volby pro nastavení SSL, ověřování na základě certifikátů klienta, šifrování pro uložené přihlašovací údaje, DoS zabezpečení a omezení IP adres. Další informace o aspektech zabezpečení najdete v následující dokumentu [Konfigurace zabezpečení rozdělit hromadné korespondence](sql-database-elastic-scale-split-merge-security-configuration.md).

Výchozí nasazené spuštěn jeden pracovní a jeden web rolí. Každá používá A1 OM velikost v Azure Cloud Services. Při nastavení nelze změnit po nasazení balíčku, je možné je změnit po úspěšné zavedení v pracovního cloudové služby, (portálu Azure). Všimněte si, že roli pracovního nesmí být nakonfigurované více než jedna instance technických důvodů. 

**Integrace shard mapy**

Služby rozdělit sloučení spolupracuje s mapou shard aplikace. Při použití služby rozdělit sloučení rozdělení nebo spojíte oblastí nebo přesouvání shardlets mezi shards, službu uchovává mapy shard aktuální. K tomu, služba připojuje k databázi shard mapy správce aplikace a udržuje oblastí a mapování průběh žádosti o rozdělení/hromadné korespondence nebo přesunout. Zajistíte tím, že mapy shard vždy nabídne vám aktuální zobrazení při se děje operace sloučení rozdělení. Rozdělení, jsou operace sloučení a shardlet pohyb implementovaná přesunutím dávku shardlets z shard zdroje na cílové shard. Během shardlet operaci přesunutí shardlets vyměřené poplatky za aktuální dávky jsou označeny jako offline v mapě shard a pro připojení směrování závislé dat pomocí **OpenConnectionForKey** rozhraní API nejsou dostupné. 

**Konzistentní shardlet připojení**

Po spuštění přesun dat pro novou dávku shardlets všechny shard mapování dodávané závislý na datech směrování připojení shard ukládání shardlet je usmrcenou a následné připojení z mapy shard rozhraní API tyto shardlets blokované Probíhá přesun dat a zabránit tak nekonzistence. Připojení k další shardlets na stejné shard taky dostat ukončit, ale bude úspěšně znovu hned na opakovat. Po přesunutí dávku shardlets jsou označeny jako online znovu shard cílové a zdrojová data se odebere z shard zdroje. Službu půjde použitím tohoto postupu pro každý list, dokud všechny shardlets byly přesunuté. To vede k několika dezaktivační operace připojení v průběhu dokončení operace rozdělit/hromadné korespondence nebo přesunout.  

**Správa dostupnosti shardlet**

Omezení připojení usmrcujících aktuální dávku shardlets jak bylo popsáno nad omezí obor nedostupnost jedna dávka shardlets najednou. Toto je upřednostňované přes přístupu k kde dokončení shard zůstane offline pro všechny shardlets v průběhu operace sloučení nebo rozdělení. Velikost listu rozumí počet různých shardlets přesunout najednou, je parametr konfigurace. Platí pro každou akci rozdělení a sloučení podle potřeby dostupnost a výkonu aplikace. Všimněte si, že oblast uzamčeného v mapě shard nesmí být větší než zadanou dávku velikost. Je to proto službu vybere rozsah velikost tak, aby se skutečný počet sharding klíčových hodnot v datech přibližně neodpovídá velikosti dávku. Toto je důležité si zapamatovat, zejména pro řídce vyplněné sharding klíče. 

**Úložiště metadat**

Udržovat její stav a zachovat protokoly během zpracování požadavku využívá služba sloučení rozdělit databázi. Uživatel vytvoří databázi ve své předplatné a poskytuje připojovací řetězec pro ni v souboru konfigurace nasazení služeb. Správci od uživatele organizace můžete taky připojení k databázi a zkontrolujte žádost o průběhu prošetřit podrobné informace týkající se potenciální k chybám.

**Plánování informovanosti sharding**

Služby rozdělit sloučení rozlišuje mezi (1) sharded tabulkami, (2) referenční tabulky a (3) normální tabulky. Sémantiku operace rozdělit/hromadné korespondence nebo přesunout, závisí na typu tabulce použité a je definována následujícím způsobem: 

* **Sharded tabulkami**: rozdělení, sloučit a operací přesunutí přesunutí shardlets ze zdroje cílové shard. Po úspěšném dokončení celkové žádosti o těchto shardlets existují už není ve zdroji. Všimněte si, že cílové tabulky musí být v cílové shard a nesmí obsahovat data v oblasti cílové před zpracování operace. 

* **Referenční tabulky**: pro referenční tabulky, rozdělení, sloučit a přesunutí operace zkopírujte data ze zdroje shard cílové. Všimněte si, že žádné změny setkáte na cílovém shard pro danou tabulku libovolného řádku již je v této tabulce v cílovém. V tabulce musí být prázdné pro všechny referenční tabulky kopírování získat zpracovány.

* **Ostatní tabulky**: jiných tabulkách můžete měly tvořit na zdrojového nebo cílového operace rozdělení a sloučení. Služby rozdělit sloučení ignoruje v této tabulce pro přesun dat nebo operace kopírování. Všimněte si, ale může rušit tyto operace pro nečekané omezení.

Informace o odkaz porovnání sharded tabulkami pochází od **SchemaInfo** rozhraní API shard mapa. Následující příklad ukazuje použití těchto rozhraní API na dané shard mapy správce objekt smm: 

    // Create the schema annotations 
    SchemaInfo schemaInfo = new SchemaInfo(); 

    // Reference tables 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "region")); 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "nation")); 

    // Sharded tables 
    schemaInfo.Add(new ShardedTableInfo("dbo", "customer", "C_CUSTKEY")); 
    schemaInfo.Add(new ShardedTableInfo("dbo", "orders", "O_CUSTKEY")); 

    // Publish 
    smm.GetSchemaInfoCollection().Add(Configuration.ShardMapName, schemaInfo); 

Tabulky "oblast" a "státní" rozumí referenční tabulky a budou zkopírovány s operacemi rozdělit/hromadné korespondence nebo přesunout. "zákazník" a "objednávek" zase se definují takhle sharded tabulkami. C_CUSTKEY a O_CUSTKEY sloužit jako klíč sharding. 

**Referenční integrita**

Služby rozdělit sloučení analyzuje závislosti mezi tabulkami a fáze operace pro přesunutí tabulky odkaz a shardlets pomocí relace cizích klíčů primární klíč. Obecně referenční tabulky se zkopírují nejdřív v pořadí závislost shardlets zkopírují v pořadí podle jejich závislosti v rámci každé dávku a pak. To je nutné, aby se při příchodu nových dat se uplatní cizího klíče PK omezení shard cílové. 

**Shard mapy konzistenci a případné dokončení**

Přítomnosti chyby služby rozdělit sloučení obnoví operace za všechny výpadku a chce dokončení všech v žádosti o průběhu. Však může existovat obnovit situací, například při shard cílové dojde ke ztrátě nebo ohroženo za opravit. Za těchto podmínek některé shardlets, které byly by měl být přesunutý nadále jsou umístěny na shard zdroje. Služba zajistí, že jsou shardlet mapování pouze aktualizovány po nezbytných dat byly úspěšně zkopírovány do cíle. Shardlets pouze odstraňují ve zdroji po jejich data byla zkopírována do cíle a odpovídající mapování jsme aktualizovali úspěšně. Operace odstranění se stane v pozadí během oblast už je online na cílovém shard. Služby rozdělit sloučení vždy zaručuje správnost mapování uložené v mapě shard.


## <a name="the-split-merge-user-interface"></a>Rozdělení sloučení uživatelského rozhraní

Sada služby rozdělit sloučení zahrnuje roli pracovní a roli web. Role webového slouží k odeslání požadavků na rozdělený sloučení interaktivní způsobem. Hlavní součásti uživatelského rozhraní se následujícím způsobem:

-    Typ operace: Typ operace je přepínač, který určuje typ operace služby pro tuto žádost. Můžete vybrat mezi rozdělení, sloučit a přesouvat scénáře. Můžete taky zrušit dříve odeslaných operace. Pomocí rozdělení, sloučit a přesunutí žádosti o oblast shard mapy. Seznam shard mapy pouze operací přesunutí podpory.

-    Mapa shard: Dál parametrů požadavku vysvětlovat, informace o shard mapy a databázi hostingu shard mapu. Zejména budete muset zadat název databáze SQL Azure server a databázi hostingu shardmap, přihlašovací údaje pro připojení k databázi shard mapy a nakonec název shard mapy. Operace v současné době přijímá pouze jednu sadu přihlašovacích údajů. Musí mít dostatečná oprávnění k provedení změn mapy shard taky tak, aby uživatelská data na shards těchto přihlašovacích údajů.

-    Zdrojové oblasti (rozdělení a sloučení): operace rozdělení a sloučení zpracuje oblasti pomocí klávesy jeho minimum a maximum. Chcete-li operaci neomezené klíčové hodnotě vysoké, zaškrtněte políčko "je maximální vysoké klíčem" a vysoké klíčové pole nechte prázdné. Oblast hodnoty klíče, které zadáte nemusí přesně shodují mapování a jeho omezení v mapě shard. Pokud nezadáte vůbec libovolnou hranici rozsahu službu automaticky odvodit nejbližší oblast. Skript Powershellu GetMappings.ps1 slouží k načtení aktuální mapování v dané shard mapu.

-    Chování zdroj úkol rozdělit (rozdělení): rozdělení operace definovat přejděte na příkaz rozdělit na zdrojovou oblast. To uděláte zadáním klávesu sharding místo, kam chcete rozdělit. Použití přepínací tlačítko zadejte zda chcete dolní části oblasti (s výjimkou klávesu rozdělení) přesunout nebo jestli chcete horní části přesunout (včetně klávesu rozdělení).

-    Zdroje Shardlet (přesunout): přesunutí operace jsou různých operacích, které rozdělení nebo sloučení nevyžadují oblasti popisuje zdroji. Zdroj pro přesunutí je určen pouze hodnoty klíče sharding, který chcete přesunout.

-    Cíl Shard (rozdělení): po zadaných informací ve zdrojovém operace rozdělení je třeba definovat místo, kam chcete data zkopírovat poskytnutím databáze SQL Azure server a databázi názvu cíle.

-    Cílový rozsah (hromadné korespondence): operace sloučení přesunutí shardlets existující shard. Existující shard určíte zadáním hranici rozsahu existující oblast, kterou chcete sloučit s.

-    Velikost dávky: Velikost dávky řídí počet shardlets, která budou přesměrovány během přesun dat v režimu offline. Toto je celočíselnou hodnotu, kde se dají používat nižšími hodnotami při jsou citlivé na dlouhý období výpadek shardlets. Vyšší hodnoty zvětšíte čas dané shardlet offline ale může zajistit zlepšení výkonu.

-    Id operace (zrušit): Pokud máte probíhající operaci, kterou už nepotřebujete, můžete operaci zrušit zadáním jeho ID operace v tomto poli. ID operace můžete načíst z tabulky žádost o stavu (viz oddíl 8.1) nebo z výstupu ve webovém prohlížeči, kde jste odeslali žádost.


## <a name="requirements-and-limitations"></a>Požadavky a omezení 

Aktuální provádění služby rozdělit sloučení podléhá následující požadavky a omezení: 

* Shards muset existují a zaregistrovat v mapě shard předtím, než lze provést operaci sloučení rozdělit na tyto shards. 

* Služba nevytvoří tabulek nebo jiných databázových objektů automaticky jako součást operace. To znamená, že schéma pro všechny sharded tabulkami a referenční tabulky existuje v cílové shard před všechny operace rozdělit/hromadné korespondence nebo přesunout. Sharded tabulkami zejména musí být prázdné v oblasti, kde jsou nové shardlets přidat operace rozdělit/hromadné korespondence nebo přesunout. V opačném neuvedete počáteční konzistence na cílovém shard. Navíc nezapomeňte tento odkaz, který data zkopírována pouze pokud odkaz je tabulka prázdná a jestli list bez konzistence záruky s ohledem na ostatní souběžné operace zápisu na referenční tabulky. Doporučujeme to: při spuštění operace rozdělit/korespondence, žádné další operace zápisu změny referenční tabulky.

* Služba závisí na řádku identity stanovených jedinečný index nebo klávesy, která zahrnuje sharding klíč pro zvýšení výkonu a spolehlivosti pro velké shardlets. Díky službu pro přesun dat o i lepší rozlišení, než jenom sharding hodnoty klíče. Díky zmenšit maximální velikost protokolu místa a zámků webů, které jsou potřeba během operace. Zvažte vytvoření jedinečný index nebo primární klíč, pokud chcete použít tuto tabulku s žádostí o rozdělení/hromadné korespondence nebo přesunutí včetně klávesu sharding na danou tabulku. Z hlediska výkonu sharding klíč by měl být počáteční sloupce v klíči nebo index.

* V průběhu zpracování žádostí o mohou být některá data shardlet prezentovat na zdrojový a cílové shard. Toto je nezbytné k zajištění proti chybám při přesunu shardlet. Integrace rozdělit korespondence s mapou shard zaručuje připojení regresi datových závislá směrování rozhraní API metodou **OpenConnectionForKey** na mapě shard nezobrazí žádné nekonzistentní intermediate státy. Však při připojování k zdrojového nebo cílového shards bez použití metodu **OpenConnectionForKey** nekonzistentní intermediate státy nemusí být vidět při žádosti o rozdělení/hromadné korespondence nebo přesunutí se děje. Tato připojení může zobrazit částečné nebo duplicitní výsledky podle toho, času nebo shard základní připojení. Toto omezení aktuálně obsahuje připojení pružná měřítko Shard více-dotazy, které udělali.

* Databázi metadat pro službu rozdělit sloučení nesmí sdílet mezi různé role. Například roli služby rozdělit sloučení spuštěné v pracovních potřeb tak, aby ukazovaly k databázi jiné metadat než roli výroby.
 

## <a name="billing"></a>Fakturace 

Služba rozdělit sloučení spuštěna jako do cloudové služby v předplatném Microsoft Azure. Proto služby cloudu poplatky instanci služby. Pokud často provádět operace rozdělit/hromadné korespondence nebo přesunout, doporučujeme, abyste že odstraníte rozdělit sloučení cloudové služby. Která ukládá nákladů pro spuštění nebo používaný instancí služby cloudu. Můžete znovu nasazení a začněte konfiguraci snadno spustitelného potřeby můžete provádět operace sloučení nebo rozdělení. 
 
## <a name="monitoring"></a>Sledování 
### <a name="status-tables"></a>Stav tabulek 

Rozdělení sloučení služba poskytuje tabulce **RequestStatus** v metadatech uložení databáze pro sledování dokončených a probíhající žádostí o. V tabulce jsou uvedeny řádku pro každý požadavek rozdělit sloučení odeslání na tuto instanci služby sloučení rozdělení. Nabízí pro každou žádost o následující informace:

* **Časové razítko**: čas a datum zahájení žádosti.

* **OperationId**: identifikátor GUID, které jednoznačně identifikuje žádost. Tento požadavek lze také operaci zrušit, i když je pořád probíhající.

* **Stav**: aktuální stav požadavku. Průběžné požadavků v něm také aktuální fáze, ve kterém je žádost o.

* **CancelRequest**: Příznak, který označuje, zda se zrušilo žádost.

* **Průběh**: odhad procento dokončení operace. Hodnota 50 označuje, že operace přibližně 50 % dokončení.

* **Podrobnosti o**: hodnota ve formátu XML, která poskytuje podrobnější sestavy průběhu. Sestavy průběhu se aktualizuje pravidelně jako sady řádků ze zdroje zkopírovány do cíle. V případě chyby nebo výjimky také v tomto sloupci podrobnější informace o chybě.


### <a name="azure-diagnostics"></a>Azure diagnostiky

Diagnostika Azure založené na Azure SDK 2,5 pro monitorování a diagnostice využívá služba rozdělit hromadné korespondence. Konfigurace diagnostiky ovládáte způsobem popsaným v tomto poli: [Povolení Diagnostika v Azure cloudovými službami a virtuálních počítačích](../cloud-services/cloud-services-dotnet-diagnostics.md). Stáhnout balíček obsahuje dvě Diagnostika konfigurace – jeden web rolí a druhý pracovní rolí. Tyto konfigurace diagnostických nástrojů pro službu podle návodu z [Cloudové služby základní informace o v Microsoft Azure](https://code.msdn.microsoft.com/windowsazure/Cloud-Service-Fundamentals-4ca72649). Definice přihlásit výkonnosti služby IIS protokoly, protokoly událostí systému Windows a protokoly událostí aplikace rozdělit hromadné korespondence zahrnuje. 

## <a name="deploy-diagnostics"></a>Nasazení diagnostických nástrojů 

Povolit sledování a diagnostice pomocí diagnostických konfigurace pro web a pracovní role poskytovanou balíček NuGet, spusťte následující příkazy pomocí Azure Powershellu: 

    $storage_name = "<YourAzureStorageAccount>" 
    
    $key = "<YourAzureStorageAccountKey" 
    
    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key  
    
    
    $config_path = "<YourFilePath>\SplitMergeWebContent.diagnostics.xml" 
    
    $service_name = "<YourCloudServiceName>" 
    
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWeb" 
    
    
    $config_path = "<YourFilePath>\SplitMergeWorkerContent.diagnostics.xml" 
    
    $service_name = "<YourCloudServiceName>" 
    
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWorker" 

Můžete najít další informace o tom, jak konfigurovat a nasazení nastavení diagnostických nástrojů tady: [Povolení Diagnostika v Azure cloudovými službami a virtuálních počítačích](../cloud-services/cloud-services-dotnet-diagnostics.md). 

## <a name="retrieve-diagnostics"></a>Načtení diagnostických nástrojů 

K vaší diagnostiky měli snadný přístup z Visual Studio Server Azure část stromu Průzkumník serveru. Otevřete instanci aplikace Visual Studio a v řádku nabídek klikněte na zobrazení a Průzkumník serveru. Klepněte na ikonu Azure připojit k předplatnému Azure. Přejděte na Azure -> úložiště -> <your storage account> -> tabulek -> WADLogsTable. Další informace najdete v tématu [Procházení prostředků úložiště v Průzkumníkovi serveru](http://msdn.microsoft.com/library/azure/ff683677.aspx). 

![WADLogsTable][2]

WADLogsTable zvýrazněná na obrázku výše obsahuje podrobné události z protokolu aplikace služby rozdělit hromadné korespondence. Všimněte si, že je výchozí konfigurace balíček stažený zaměřené provozní nasazení. Tedy interval niž protokoly a čítačů doplněné z instancí služby velké (5 minut). Pro testování a vývoj Ztlumením interval úpravou nastavení diagnostických nástrojů webu nebo pracovního role vašim potřebám. Klikněte pravým tlačítkem myši na roli v Průzkumníku serveru Visual Studio (viz výše) a potom upravit přepojení v dialogovém okně Konfigurace nastavení diagnostických nástrojů: 

![Konfigurace][3]


## <a name="performance"></a>Výkon

Obecně je lepší výkon očekávané vyšší, další performant služby úrovní v databázi SQL Azure. Vyšší vstupu a výstupu, procesoru a paměti rozdělení pro vyšší úrovně služby prospěch hromadného kopírování a odstraňte operacích, které využívá služba rozdělit hromadné korespondence. Z tohoto důvodu rozšiřte vrstvy služeb jenom pro tyto databáze pro definované, omezené časové období.

Služba také provádí ověření dotazy v rámci jeho normálně. Tyto dotazy ověření zjišťovat neočekávané přítomnost dat v oblasti cílové a ujistěte se, že všechny operace rozdělit/hromadné korespondence nebo přesunout začíná konzistentního stavu. Všechny dotazy práci během sharding klíčové oblasti definované rozsah operaci a velikost dávky zadané jako součást definici žádost. Tyto dotazy provádí nejlépe index je prezentace, která má klávesu sharding jako počáteční sloupec. 

Kromě toho jedinečnost vlastnost s klávesou sharding jako počáteční sloupec vám umožní službu optimalizované možnost, která omezuje využití prostředků z hlediska prostoru protokolu a paměti používat. Tato vlastnost jedinečnost je potřeba k přesunutí velikost velkých dat (obvykle nad 1GB). 

## <a name="how-to-upgrade"></a>Jak upgradovat

1. Postupujte podle kroků v [nasadit služby rozdělit sloučení](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
2. Změna konfiguračního souboru cloudové služby pro nasazení rozdělit sloučit tak, aby odrážely nové konfigurační parametry. Nový potřeba parametr jsou informace o certifikátu použitém k šifrování. Snadný způsob, jak můžete to udělat je a umožňuje porovnávat nový soubor šablony konfigurace z stahování proti existující konfiguraci. Zkontrolujte, že přidáte nastavení "DataEncryptionPrimaryCertificateThumbprint" a "DataEncryptionPrimary" pro web a roli kolegy.
3. Před nasazením aktualizace Azure, ověřte, že byl dokončen všechny operace probíhající rozdělit korespondence. Můžete snadno provést pomocí dotazu RequestStatus a PendingWorkflows tabulek v databázi rozdělit sloučení metadat pro probíhající požadavky.
4. Aktualizujte stávajícího nasazení služby cloudu pro rozdělení korespondence předplatné Azure nový balíček a konfigurační soubor aktualizovaných služeb.

Není potřeba vytvořit novou databázi metadat pro rozdělení korespondence upgradovat. Novou verzi automaticky upgraduje stávající databázi metadat pro nové verze. 

## <a name="best-practices--troubleshooting"></a>Osvědčené postupy a řešení potíží
 
-    Definice test klienta a výkonu svého nejdůležitější operací rozdělit/sloučení/přesunutí s klienta test napříč několika shards. Zajistit, že všechny metadat je definován správně v mapě shard a operace omezení nebo cizích klíčů není porušovat.
-    Zachovat klienta test velikost dat nad velikostí maximální datového tenantovi od největšího k zajištění nejsou výskytu velikost dat související s jejich stavem. Pomůže vám to posuďte horní mez na časové náročnosti přemístíte jednoho klienta. 
-    Zkontrolujte, že vaše schéma umožňuje odstraněné položky. Služba rozdělit sloučení vyžaduje možnost po data byly úspěšně zkopírovány do cíle: Odeberte data ze zdroje shard. Například **Odstranění aktivační události** můžete zabránit, aby služba odstranění dat ve zdroji a tuto chybu může způsobovat operace selhání.
-    Sharding klíč by měl být počáteční sloupce v definici jedinečný index nebo primární klíč. Která zaručuje nejlepší možný výkon pro ověření dotazy sloučit nebo rozdělit a vlastních dat pohybu a odstranění operací, které pracují vždy pro sharding klíčové oblasti.
-    Společně umístit služby rozdělit korespondence v centru oblast a dat, kde jsou uloženy databáze. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]



<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-overview-split-and-merge/split-merge-overview.png
[2]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics.png
[3]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics-config.png
 
