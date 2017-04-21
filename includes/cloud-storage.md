#<a name="data-management-and-business-analytics"></a>Správa dat a analýzy Business

Správa a analýza dat v cloudu je stejně důležité je to někde jinde. Pokud chcete, aby vám to, Azure obsahuje velké množství technologie pro práci s daty relační a -relační. Tento článek uvádí jednotlivé možnosti. 

##<a name="table-of-contents"></a>Obsahy      

- [Úložiště objektů BLOB](#blob)
- [Spuštění systému správy databáze v počítači virtuální](#dbinvm)
- [Databáze SQL](#sqldb)
    - [Synchronizace dat SQL](#datasync)
    - [SQL dat k vytváření sestav pomocí virtuálních počítačích](#datarpt)
- [Úložiště tabulek](#tblstor)
- [Hadoop](#hadoop)

## <a name="blob"></a>Úložiště objektů BLOB

Slovo "objektů blob" je zkratka pro "binární velké objekt" s popisem přesně je jaké blob: kolekce binární informace. Ještě i když jsou jednoduché, objekty BLOB jsou velmi užitečné. [Obrázek 1](#Fig1) znázorňuje základní informace o úložišti objektů Blob Azure.

<a name="Fig1"></a>![Diagram objekty BLOB][blobs]
 
**Obrázek 1: Úložiště objektů Blob Azure ukládá binárními daty – objektů BLOB - kontejnery.**

Použití objektů BLOB, nejprve vytvořit Azure *účtu úložiště*. Jako součást této zadáte Azure datacentra, které se uloží objekty, které vytvoříte pomocí tohoto účtu. Místo, kde je umístěn každý objektů blob vytvořená patří některé kontejneru ve vašem účtu úložiště. Získat přístup objektů blob aplikace poskytuje adresu URL s formuláři:

http://&lt;*StorageAccount*&gt;.blob.core.windows.net/&lt;*kontejneru*&gt;/&lt;*BlobName*&gt;

&lt;*StorageAccount* &gt; je jedinečný identifikátor přiřazený po vytvoření nového účtu úložiště během &lt; *kontejneru* &gt; a &lt; *BlobName* &gt; jsou názvy kontejneru konkrétní a objektů blob v kontejneru. 

Azure poskytuje dva různé typy objektů BLOB. Možnosti jsou:

- Objekty BLOB *blok* , z nichž každá může obsahovat maximálně 200 GB data. Jak naznačuje název bloku objektů blob rozdělit na určitý počet bloků. Pokud dojde k selhání při převodu blok objektů blob, přenosu můžete pokračovat posledních blok neposílejte celý objektů blob znovu. Objekty BLOB bloku jsou úplně obecný přístup k základnímu úložišti a budou typ nejčastěji používaná objektů blob dnes.

- Objekty *stránky* BLOB, které může být v jedné terabajt. Objekty BLOB stránky jsou sice pro RAM a tak každého z nich je rozdělen do některé počet stránek. Aplikace je zadarmo pro čtení a zápis jednotlivé stránky náhodně v objektů blob. Ve virtuálních počítačích Azure, například VMs vytvoříte pomocí objektů BLOB stránky jako trvalý úložiště disků OS a disků data.

Jestli se rozhodnete objektů BLOB blok nebo stránku objektů BLOB, aplikací datům můžete přistupovat objektů blob několika různými způsoby. Možnosti patřit následující úkoly:

- Přímým RESTful (tedy protokolu HTTP) přístup k protokolu. Azure aplikací a externích aplikacích, včetně aplikace spuštěné místně, můžete použít tuto možnost.
- Pomocí knihovny Azure úložiště klienta, který poskytuje rozhraní více vývojář ovládáním nad nezpracovanými RESTful objektů blob access Protocol (protokol). Ještě jednou Azure aplikací a externích aplikacích přístup k objektů BLOB pomocí této knihovny.
- Použití Azure, se nedá považovat na místní disk s systém soubor NTFS stránky objektů blob Azure aplikace. S aplikací vypadá objektů blob stránky běžnému systému souborů Windows otevřít prostřednictvím vstup/výstup standardní souboru. Ve skutečnosti čte a zapisuje se odesílají základní stránka objektů blob implementující jednotku Azure. 

Chránit před selhání hardwaru a zlepšit dostupnost, je každý objektů blob replikovat přes tři počítače v Azure datacentra. Psaní objektů blob aktualizuje všechny tři kopie, takže novější čtení neuvidíte nekonzistentní výsledky. Můžete taky určit, že mají být objektů blob data zkopírována do jiného Azure datacentra ve stejné geo ale aspoň 500 mil jako pryč. Kopírovat, s názvem *geo replikace*se stane objevit během pár minut aktualizaci objektů blob a slouží k obnovení havárie.

Data v objektů BLOB provést také k dispozici prostřednictvím Azure *Obsahu doručení Network (CDN)*. Uložením do mezipaměti kopií objektů blob datům desítky servery ve světě, můžete CDN dosažení vyššího přístup na informace, které se k nim získat přístup opakovaně. 

Jednoduchý jsou objekty BLOB jsou volbou v řadě situací. Ukládání a přenos videa a zvuku příkladů zřejmé, jako jsou zálohování a jiné druhy archivace dat. Můžete taky vývojáři objektů BLOB podržte jakýkoli druh Nestrukturovaná data, která se líbí. Máte jednoduchý způsob, jak ukládat a používat binární data můžete využít překvapivě.


## <a name="dbinvm"></a>Spuštění systému správy databáze v počítači virtuální

Velké množství aplikací dnes závisí na určitý druh systému správy databáze (systému správy databáze). Relační systémy, třeba SQL Server jsou nejčastěji používané volba, ale -relační přístupy známý jako *NoSQL* technologií, budete moct použít více Oblíbené každý den. Jestli Pokud chcete, aby aplikace cloudu pomocí těchto možností správy dat Azure virtuálních počítačích vám umožní pracovat systému správy databáze (relační nebo NoSQL) do virtuálního počítače. [Obrázek 2](#Fig2) ukazuje, jak to vypadá se serverem SQL Server.

<a name="Fig2"></a>![Diagram SQL Server ve virtuálních počítače][SQLSvr-vm]
 
**Obrázek 2: Azure virtuálních počítačích umožňuje aplikaci systému správy databáze OM, s trvalé poskytovanou objektů BLOB.**

Vývojáři i správci databáze tento scénář vypadá podobně softwarem stejné ve své vlastní datacentra. V tomto příkladu jako je například téměř funkcí SQL serveru lze použít a mít oprávnění Úplné správce systému. Máte taky zodpovědnost správy databázový server samozřejmě stejně, jako kdyby ho používali místně.

Jak ukazuje [Obrázek 2](#Fig2) , se zobrazí uložený na místním disku OM serveru běží ve vašich databází. V části titulní stránky ale každý z těchto discích je došlo k zápisu objektů blob Azure. (Je podobný pomocí SAN ve vlastní datacentru objektů blob budou sloužit podobně jako logické jednotky.) S všech objektů blob Azure data, která obsahuje je replikovat třikrát v rámci datacentra a pokud požadujete ho geo replikovat na jiné datacentra ve stejné oblasti. Také je možné použít možnosti například odrážející strukturu pro Zvýšená spolehlivost: databáze SQL serveru. 

Jiným způsobem použití systému SQL Server ve virtuálního počítače je k vytvoření hybridního aplikace, kde data jsou umístěná na Azure místní spuštěn logiky aplikace. Například to může být vhodné při spuštěné do několika umístění nebo na různých mobilních zařízení musí vzájemně sdílet stejná data. Aby komunikaci mezi cloudu databáze a místní logickou jednodušší organizace umožňuje virtuální sítě Azure vytvořit virtuální privátní sítě (VPN) spojení mezi Azure datacentra a vlastní místní datacentra.


## <a name="sqldb"></a>Databáze SQL

Pro mnoho lidí spuštění systému správy databáze do virtuálního počítače je první možnost, která vás napadne pro správu strukturovaných dat v cloudu. To je to není jenom volba, však ani je vždy o nejlepší variantu. V některých případech Správa dat pomocí platformu jako přístup služby (PaaS) větší smysl. Azure poskytuje PaaS technologií SQL databáze, které můžete to udělat pro relačních dat s názvem. [Obrázek 3](#Fig3) ukazuje tuto možnost. 

<a name="Fig3"></a>![Diagram databáze SQL][SQL-db]
 
**Obrázek 3: SQL databáze obsahuje sdílených služeb relační úložiště PaaS.**

Databáze SQL nedáte jednotlivé zákazníky vlastní pole fyzicky instanci systému SQL Server. Místo toho poskytuje více klienta služby logické databáze SQL serveru pro jednotlivé zákazníky. Všechny zákazníky sdílet kapacitu úložiště a výpočetním tato služba poskytuje. A s úložiště objektů Blob všechna data do SQL databáze uložena tři samostatné počítačů Azure datacentru, pojmenování databáze předdefinované vysoké dostupnosti (HA).

Aplikace databáze SQL vypadá podobně SQL serveru. Aplikace můžete problém SQL dotazů relační tabulky použít T-uložené procedury SQL a provádějí transakce v několika tabulkách. A protože aplikace přístup k databázi SQL pomocí protokolu tabulkových toku dat (Neuvedeno), protokol stejné použitých při přístupu k SQL Server, můžete pracovat s daty pomocí Entity Framework, ADO.NET, JDBC a jiných známé datová rozhraní. 

Ale, a proto SQL databáze je Cloudová služba spuštěná v Azure datacentrech nemusíte spravovat libovolné fyzické aspekty systému, například disku. Můžete taky nemusíte bát aktualizace softwaru nebo zpracování jiných nízkoúrovňový úkoly správy. Každý organizaci zákazníka pořád ovládací prvky vlastní databází samozřejmě, včetně jejich schémata a přihlášení uživatele, ale spoustu rutinní úkoly správy dělají za vás. 

Během databáze SQL vypadá podobně SQL serveru k aplikacím, nemá chovat stejně jako systému správy databáze systémem fyzické nebo virtuálního počítače. Protože běží na sdíleném hardwaru, jeho výkon závisí na načíst umístěný na hardware všechny svým zákazníkům. To znamená, že výkon, vyslovte příkaz proceduru uloženou v databázi SQL se mohou lišit od jeden den na další. 

V současné době databáze SQL umožňuje vytvářet databáze podržte maximálně 150 GB. Pokud potřebujete pracovat s větší databází, tato služba poskytuje možnost *federace*. K tomuto účelu správce databáze vytvoří dva nebo více *federace členové*, přičemž každý z nich je samostatná databáze pomocí vlastní schéma. Data je rozšířit mezi tyto členy, něco, co je označovaná také jako *sharding*s nemusel(a) přiřazené jedinečný *federace klíče*. Cílem by měl aplikace problémy SQL dotazů tato data zadáním federation klíč, který identifikuje člen federation dotaz. Díky tomu pomocí velké objemy dat tradiční relační přístup. Co vždy existuje střídání; dotazy ani transakce mohou být rozloženy federace členů, například. Ale když tyto střídání jsou přijatelné relační služby PaaS se o nejlepší variantu, pomocí SQL Federation může být vhodné řešení.

Databáze SQL může používat spuštěné v Azure nebo kdekoli jinde, jako je třeba v místním datacentra. To je užitečné pro cloudové aplikace, které potřebují relačních dat, stejně jako místní aplikace, které můžete využít ukládání dat v cloudu. Mobilní aplikace může používat SQL databáze pro správu sdílené relačních dat, například jako může zásob aplikace, která poběží na více prodejci celém světě.

Přemýšlet o databáze SQL vyvolá zřejmé (a důležité) problém: když by měl spustíte SQL serveru v virtuálního počítače a kdy je databáze SQL lepší volbou? Jako obvykle jsou střídání a to je lepší jaký přístup závisí na tom vašim požadavkům. 

Jednoduchý způsob myslet je zobrazení SQL databáze jako u nových aplikací, když SQL Server ve virtuálního počítače je lepší volbou při přesunování existující místní aplikace do cloudu. Může být také užitečné podívejte se na toto rozhodnutí více jemně odstupňovaná způsobem, ale. Databáze SQL je například snáze používat, protože minimální nastavení a správa. Ale serverem SQL Server ve virtuálního počítače můžete mít předvídatelně výkonu – není sdílených služeb - a taky podporuje větší nefederované databáze než SQL databáze. Pořád databáze SQL nabízí předdefinované replikace dat a zpracování efektivně pojmenování systému správy vysoké dostupnosti databáze s velmi málo práce. SQL Server umožňuje větší kontrolu a poněkud širší skupinu možností, je jednodušší nastavení a výrazně méně práce pro správu databáze SQL.

Nakonec je důležité ukazující, že databáze SQL není pouze PaaS službu dat k dispozici v Azure. Partneři Microsoft poskytují i další možnosti. Například ClearDB nabízí MySQL PaaS nabízí, zatímco Cloudant prodává NoSQL možnost. Datové služby PaaS jsou správné řešení v mnoha případech a tento přístup pro správu dat je vlastně důležitou součástí Azure.


### <a name="datasync"></a>Synchronizace dat SQL

Během databáze SQL zachovat tři kopie každé databáze v rámci jedné Azure datacentru, ho není automaticky replikovat dat mezi Azure datacentrech. Místo toho poskytuje SQL dat synchronizace služby, které můžete použít k tomuto. [Obrázek 4](#Fig4) ukazuje, jak to vypadá.

<a name="Fig4"></a>![Diagram synchronizovat data SQL][SQL-datasync]
 
**Obrázek 4: Synchronizace dat SQL synchronizuje data do SQL databáze s daty v jiných Azure a místních datacentrech.**

Diagram ukazuje, můžete synchronizovat Data SQL synchronizovat data na různých místech. Předpokládejme, že používáte aplikaci ve více Azure datacentrech, například s daty uloženými v databázi SQL. Synchronizace dat SQL slouží k této synchronizovat data. Synchronizace dat SQL můžete taky synchronizovat data mezi Azure datacentra a instance serveru SQL Server spuštěné v místním datacentra. To může být užitečné pro zachování místní kopii data použitá aplikacemi místní a kopii cloudu používané aplikace spuštěné v Azure. A i když není vidět na obrázku, synchronizovat Data SQL lze také k synchronizaci dat mezi databáze a SQL serveru SQL Server spuštěné v OM Azure nebo kdekoli jinde.

Synchronizace může být obousměrné a zjistíte, přesně jaká data se synchronizuje a jak často se to dělá. (Synchronizace mezi databázemi není atomová, ale - je vždy alespoň některých prodleva.) Se však pracovní postup slouží, nastavení synchronizace se synchronizací Data SQL úplně konfigurace řízené úsilím; neexistuje žádný kód psát.


### <a name="datarpt"></a>SQL dat k vytváření sestav pomocí virtuálních počítačích

Jakmile databáze obsahuje data, někdo bude pravděpodobně chtít vytvářet sestavy pomocí tato data. Azure mohlo by umožnit spuštění služby SQL Server Reporting Services (SSRS) ve virtuálních počítačích Azure, která odpovídá funkčně systém SQL Server Reporting Services místní. Pak můžete SSRS spuštění sestavy dat uložených v databázi SQL Azure.  [Obrázek 5](#Fig5) ukazuje, jak funguje procesu.

<a name="Fig5"></a>![Diagram podávání zpráv SQL][SQL-report]
 
**Obrázek 5: SQL Server Reporting Services spuštěna virtuálních počítačích Azure poskytuje služby reporting services pro data do SQL databáze. .**

Před uživatele můžete zobrazit sestavy, někdo definuje zprávy by měl vypadat takto (krok 1). S SSRS na virtuálního počítače lze to provést pomocí jedné z dva nástroje: SQL Server Data Tools, část SQL Server 2012 nebo jeho předchůdce Development Studio Business Intelligence (BI). Stejně jako u SSRS, jsou tyto definice sestavy vyjádřený v jazyk definice sestavy (RDL). Po vytvoření souborů RDL pro sestavu jsou odeslány do OM v cloudu (krok 2). Definice sestavy je nyní připravena k použití.

Pak přístupu uživatele aplikace sestavy (krok 3). Aplikace předá tohoto požadavku SSRS v angličtině (krok 4), které kontakty SQL databáze nebo jiných zdrojů dat přístup k datům potřebuje (krok 5). SSRS používá tyto údaje a relevantní RDL soubory k vykreslování sestavy (krok 6), pak vrátí sestavy do aplikace (krok 7), které zobrazuje uživateli (krok 8).

Vložení sestavy v aplikaci, scénář příkladu, není jediná možnost. Je také možné zobrazit sestavy v správce sestav na OM na OM SharePoint nebo jinými způsoby. Sestavy lze také sloučit, jednu zprávu obsahující odkaz na jiný.

SSRS na Azure OM nabízí kompletní funkce jako podřízenosti řešení v cloudu. Sestavy můžete použít libovolný zdroje dat podporované kodérem SSRS. Aplikace a sestavy může obsahovat vloženého kódu nebo sestavení pro podporu vlastní chování. Spuštění sestavy a vykreslování jsou rychlé, protože obsah serveru sestavy a engine spustit spolupráce na stejném virtuálního serveru.



## <a name="tblstor"></a>Úložiště tabulek

Je užitečné v mnoha případech relačních dat, ale ne vždy je volbou. Pokud aplikace potřebuje rychlý a jednoduchý přístup k velmi velké objemy volně strukturovaných dat, například relační databáze nemusí fungovat dobře. Technologie NoSQL je pravděpodobně vhodnější.

Úložiště tabulek Azure je znázorněn příklad tohoto typu NoSQL přístup. Úložiště tabulek bez ohledu na jeho jméno, které nejsou podporovány standardní relační tabulky. Místo toho poskytuje takzvanou *klíč/hodnota ukládat*, přidružení množiny dat určité klávesy a pak nechat aplikace access tato data zadáním klávesu. [Obrázek 6](#Fig6) ukazuje základní informace.

<a name="Fig6"></a>![Diagram úložiště tabulek][SQL-tblstor]
 
**Obrázek 6: Úložiště tabulek Azure je úložiště klíč/hodnota, která poskytuje rychlé a snadný přístup velké objemy dat.**

Jako objekty BLOB je přidružený účet Azure úložiště každou tabulku. Tabulky jsou taky s názvem podobně jako objekty BLOB s adresou URL formuláře

http://&lt;*StorageAccount*&gt;.table.core.windows.net/&lt;*tabulky*&gt;

Jak je vidět na obrázku, každá tabulka je rozdělen do některé počet oddílů, z nichž každá mohou být uloženy na samostatném počítač. (Toto je formuláře sharding, stejně jako u SQL federace.) Azure aplikace a aplikace spuštěné jinde získat přístup pomocí protokolu RESTful OData nebo do jiné knihovny Azure úložiště klienta.

Každý oddíl v tabulce obsahuje několik počet *entity*, každý obsahující až 255 *Vlastnosti*. Každá vlastnost má název, typ (například binární, logická hodnota, data a času, Int nebo řetězec) a hodnotu. Na rozdíl od relační úložiště v této tabulce mít žádné pevné schéma a tak různých entit ve stejné tabulce může obsahovat vlastnosti s různými typy. Jedna osoba může mít jenom vlastnost řetězec obsahující název, například, zatímco jiné osobě ve stejné tabulce má dvě vlastnosti Int obsahující číslo ID zákazníka a platební hodnocení.

Aplikace k identifikaci konkrétní entity uvnitř tabulky, poskytuje klíč této entity. Klíč má dvě části: *klíč oddílu* , který identifikuje konkrétní oddíl a *klíče řádku* , který identifikuje entita v tomto oddílu. [Obrázek 6](#Fig6)například klient požaduje entita s oddíl A a řádku klíče 3 a úložiště tabulek vrátí tohoto postupu včetně všech vlastnosti, které obsahuje.

Umožňuje tuto strukturu tabulky velké – jedné tabulky může obsahovat maximálně 100 TB dat – a umožňuje rychlý přístup k datům, které obsahují. Také přináší omezení, ale. Je třeba nepodporuje žádný transakční aktualizace, které zahrnují tabulek nebo dokonce oddíly z jedné tabulky. Nastavení aktualizací do tabulky můžete jenom seskupené do atomová transakce Pokud souvisejících entity nejsou ve stejném oddílu. Zde je také žádným způsobem k vytvoření dotazu tabulku na základě hodnoty vlastností, ani není podpora spojení mezi více tabulek. Stav a na rozdíl od relační databáze, tabulek žádnou podporu pro uložené procedury.

Úložiště tabulek Azure je dobrá volba, pokud aplikace, které potřebují rychlé a levný přístup k velkému počtu volně strukturovaná data. Například aplikace Internet informacemi o profilu pro víc uživatelů pomocí tabulek. Rychlý přístup je důležité v takovém případě a aplikace pravděpodobně nevyžaduje úplné power SQL. Ukončen prvek pro tuhle funkci získat rychlost a velikost může někdy smysl a úložiště tabulek je vlastně jenom správné řešení pro některé problémy.


## <a name="hadoop"></a>Hadoop

Máte organizace vytváření pro desetiletí sklady data. Tyto kolekce informací nejčastěji uložená v tabulkách relačních, informujte lidi spolupracujete a informace z dat v mnoha různými způsoby. Se serverem SQL Server je například běžné používat nástroje jako SQL Server Analysis Services můžete to udělat.

Ale Předpokládejme, že chcete udělat analýzu-relačních dat. Data zabrat hodně formulářů: informace z senzory nebo RFID značky protokoly v serverové farmy, navštívených vytvořené pomocí webové aplikace obrázky z lékařích diagnostiky zařízení a další. Tato data může být také skutečně velký, moc velký pro efektivní tradiční datový sklad. Problémy s daty velký takto, méně častých před několika lety teď se stali úplně běžné.

Analyzovat požadovaný typ velký dat, náš odvětví převážně došel na řešení jednotného: technologie otevřít zdroj Hadoop. Hadoop spustí clusteru fyzické nebo virtuálních počítačích šíření data, která funguje na napříč těchto počítačů a zpracování souběžně. Další počítačích Hadoop má použít, tím rychleji můžou udělat něco jiného, co práce dělá.

Tento identifikovat problém je přirozený přizpůsobit pro veřejné cloudu. Namísto udržování army místní servery, které může sednout nedělá moc času spuštění Hadoop v cloudu umožňuje vytvořit (a platu pro) VMs pouze v případě, že je budete potřebovat. Ještě lépe a informace ze velký data, která chcete analyzovat s Hadoop se vytvoří v cloudu, ušetříte potíže s přesouvat. Můžete využít tyto součinnosti, Microsoft týkající se Hadoop služby Azure. [Obrázek 7](#Fig7) zobrazuje nejdůležitější komponent tuto službu.

<a name="Fig7"></a>![Diagram hadoop][hadoop]

**Obrázek 7: Hadoop na Azure spustí MapReduce úlohy, které zpracovávají dat paralelně pomocí několika virtuálních počítačích.**

Používání Hadoop na Azure, nejdřív požádáte tuto platformu cloudu vytvořit Hadoop cluster určující počet VMs potřebujete. Nastavení Hadoop clusteru je není rozsáhlé úkolu a tak nechat Azure to vám dává smysl. Až to budete mít pomocí clusteru, můžete vypnout. Je potřeba zaplatit výpočetním prostředky, které nepoužíváte.

Aplikace Hadoop běžně označovanému jako *úlohy*, používá programovací model jmenoval *MapReduce*. Obrázek ukazuje, logiky pro projekt MapReduce spustí současně napříč mnoha VMs. Zpracováním dat paralelně Hadoop můžete data analyzovat rychleji než jednom počítači řešení.

Na Azure data MapReduce úlohy označené jako pracuje na bude obvykle k dispozici v úložišti objektů blob. V Hadoop ale MapReduce úlohy očekávají, že data se uloží do *Hadoop Distributed soubor systému (HDFS)*. HDFS je podobný k úložišti objektů Blob některé způsoby; ho zreplikuje dat mezi více fyzické serverů, například. Namísto Duplikovat tuto funkci, Hadoop na Azure místo toho zpřístupňuje úložiště objektů Blob prostřednictvím rozhraní API HDFS, jak ukazuje obrázek. Když logickou MapReduce úlohy předpokládá, že ho je přístup k souborům běžnému HDFS, úlohy ve skutečnosti práce s daty streamují k němu z objektů BLOB. A na podporu případ, kde mají více úloh spustit stejnými daty, Hadoop na Azure umožňují kopírování dat z objektů BLOB do úplné HDFS aplikaci VMs. 

Úlohy MapReduce jsou běžně psaných Java dnes, přístup, který podporuje Hadoop na Azure. Společnost Microsoft taky přidala podporu pro vytváření MapReduce úlohy v jiných jazycích, včetně C# F # a JavaScript. Cílem je zpřístupňují této technologie velký dat snadněji větší skupinu vývojářů.

Spolu s HDFS a MapReduce obsahuje Hadoop jinými technologiemi, které povolení uživatelům analyzovat data bez psaní úlohy MapReduce sami. Prasátko je například uceleném jazyk vyvinutý pro analýzu dat velký, zatímco podregistru nabízí jazyka SQL profesionálové s názvem HiveQL. Prasátko a podregistru skutečně generovat MapReduce úlohy, které zpracovávají HDFS dat, ale skrýt tento složitost ze své uživatele. Jak k dispozici Hadoop na Azure.

Microsoft poskytuje také ovladač HiveQL aplikace Excel. Pomocí doplňku aplikace Excel, obchodní analytici můžete vytvořit HiveQL dotazy (a tedy MapReduce úlohy) přímo z aplikace Excel, pak procesu a vizualizovat výsledky pomocí doplňku PowerPivot a dalších nástrojů aplikace Excel. Hadoop na Azure obsahuje jinými technologiemi i, například počítače výukové knihoven Mahout systému dolování grafu Pegasus a další.

Analýza velkých dat je důležité, a Hadoop tedy taky důležité. Zadáním Hadoop jako služba spravovaných na Azure, spolu s odkazy na známé nástroje, jako je Excel, Microsoft usiluje o zpřístupnění této technologii širší skupinu uživatelů.

Data z nejrůznějších nerozhodnete, je důležité. Z tohoto důvodu Azure obsahuje celou řadu možností pro analýzu dat správu a business. Jakékoli aplikace se pokoušíte vytvořit, je pravděpodobné, že zjistíte něco v této mraků platformě, která budou fungovat za vás.

[blobs]: ./media/cloud-storage/Data_01_Blobs.png
[SQLSvr-vm]: ./media/cloud-storage/Data_02_SQLSvrVM.png
[SQL-db]: ./media/cloud-storage/Data_03_SQLdb.png
[SQL-datasync]: ./media/cloud-storage/Data_04_SQLDataSync.png
[SQL-report]: ./media/cloud-storage/Data_05_SQLReporting.png
[SQL-tblstor]: ./media/cloud-storage/Data_06_TblStorage.png
[hadoop]: ./media/cloud-storage/Data_07_Hadoop.png
