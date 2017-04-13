<properties
   pageTitle="Co je Azure SQL datový sklad? | Microsoft Azure"
   description="Podnikových distribuovaná schopný zpracovávat petabyte objemy dat relační a -relační databáze. Je odvětví první cloudu datový sklad s zvětšit, zmenšit a zastavte ukazatel myši v sekundách."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/27/2016"
   ms.author="lodipalm;barbkess;mausher;jrj;sonyama;kevin"/>


# <a name="what-is-azure-sql-data-warehouse"></a>Co je Azure SQL datový sklad?

Azure SQL datový sklad je cloudové, škálování databáze může zpracování rozsáhlé objemy dat, relační i -relační. Založená na naše architektura datových souběžné zpracování (MPP), SQL datový sklad zpracování pracovního vytížení vaší organizace.

Datový sklad SQL:

- Sloučí relační databáze systému SQL Server s možnostmi škálování Azure cloudu. Můžete zvýšit, zmenšit, pozastavit nebo obnovit výpočetním v sekundách. Můžete ušetřit náklady rozšiřování procesoru kdykoliv ho budete potřebovat a vyjmutí zpět použití špičce není.
- Využití platformu Azure. Nasazení je snadné, Bezproblémová zachována a plně odolnost proti chybám kvůli automatické abstinenčním zpět.
- Doplňuje SQL Server zahrnující. Můžete vyvíjet pomocí nástrojů a známé SQL Server Transact-SQL (T-SQL).

Tento článek popisuje hlavní funkce SQL datový sklad.

## <a name="massively-parallel-processing-architecture"></a>Architektura datových paralelní zpracování

SQL datový sklad je že datových souběžné zpracování (MPP) rozdělením databáze systému. Rozdělením dat a zpracování funkce v několika uzlech, nabízí SQL datový sklad velké škálovatelnost – Pokud za jeden systém.  Na pozadí SQL datový sklad rozložení dat přes mnoha sdílené není nic úložiště a zpracování jednotky. Data se uložené v úložišti místně nadbytečné Premium a odkazovaného pro výpočet uzly pro spuštění dotazu. Tato architektura SQL datový sklad trvá, než "dělení a dobytí" přístup službou zatížení a složitých dotazů. Žádosti o jsou dostali uzel ovládacího prvku, optimalizované a pak předána uzly výpočetním pracovat současně.

Zkombinováním MPP architektura a možnosti ukládání Azure SQL datový sklad provést tyto akce:

- Zvětšit nebo zmenšit nezávisle na využití úložiště.
- Zvětšit nebo zmenšit výpočetním bez přesunutí dat.
- Pozastavit výpočet kapacita zachováním dat beze změny.
- Životopis výpočet kapacita v oznámení okamžiku.

Na následujícím obrázku vidíte architektura podrobněji.

![Architektura sklad dat SQL][1]


**Uzel ovládacího prvku:** Ovládací prvek uzel spravuje a optimalizuje dotazů. Je front-end, který spolupracuje s všechny aplikace a připojení. V SQL datový sklad uzel ovládacího prvku technologii databáze SQL a připojení k němu vypadá a funguje stejně. V části povrchový souřadnic uzel ovládacího prvku všechny přesun dat a výpočtu potřebné ke spuštění paralelní dotazy na data distribuované. Po odeslání dotazu T-SQL pro SQL datový sklad převede jej uzel ovládacího prvku do samostatné dotazy spuštěné v operačním systému každý výpočetní uzly souběžně.

**Výpočet uzly:** Uzly výpočetním sloužit jako power za SQL datový sklad. Jsou databáze SQL, který ukládání dat a zpracování dotazu. Po přidání dat SQL datový sklad distribuuje do výpočetního uzly řádky. Uzly výpočetním jsou zaměstnanců spuštěné v operačním systému dat parallel dotazů. Po zpracování předání výsledků zpět do uzel ovládacího prvku. Dokončete dotaz uzel ovládacího prvku sloučí výsledky a vrátí výsledek.

**Úložiště:** Data uložena v úložišti objektů Blob Azure. Uzly výpočetním pracovat s daty, psaní a číst přímo do a z úložiště objektů blob. Vzhledem k tomu Azure úložiště rozbalí transparentně a SQL datový sklad výrazně, umí totéž. Protože jsou nezávislé výpočetním a úložiště, SQL datový sklad můžete automaticky změnit velikost úložiště odděleně od měřítko výpočetním a naopak. Úložiště objektů Blob Azure také plně odolnost proti chybám a zjednodušuje procesu zálohování a obnovení.

**Datové služby pohybu:** Data pohyb služby správy přesune data mezi uzly. Systému správy dokumentů dává přístup uzly výpočetním dat, které potřebují pro spojení a agregace. Systému správy dokumentů není službou Azure. Je služba systému Windows, která běží vedle SQL databáze ve všech uzlů. Protože systému správy dokumentů běží na pozadí, nebude interakce s přímo. Ale když se podíváte na plány dotazů, zjistíte, že jsou to například některé operace systému správy dokumentů, protože přesun dat je nutné spustit obou dotazů paralelně.


## <a name="optimized-for-data-warehouse-workloads"></a>Optimalizovaná pro datový sklad úloh

Přístup MPP podporovaných počet datových skladování optimalizaci výkonu konkrétní, včetně:

- Optimalizace distribuované dotazů a nastavte s jednou variantou složité přes všechna data. Používání informací na velikost dat a distribuce, služba není možné optimalizovat dotazy podle hodnocení nákladů konkrétní distribuované dotazu.

- Rozšířené algoritmů a postupy integrovat do procesu dat pohyb efektivní přesun dat mezi výpočetních zdroje podle potřeby provádět dotaz. Tyto operace pohyb dat jsou součástí a všechny optimalizace ke službě pohyb dat dojít automaticky.

- Skupinový **columnstore** indexy ve výchozím nastavení. Pomocí úložiště založená na sloupec SQL datový sklad získá průměrně 5 zisky komprese x přes tradiční úložiště řádkovou orientaci a až 10 x nebo další zlepšení výkonu dotazu. Analýzy dotazů, které je potřeba zkontrolovat velkého počtu řádků práce skvěle i na columnstore indexy.


## <a name="predictable-and-scalable-performance"></a>Výkon a škálovatelnost dosáhnete

SQL datový sklad odděluje úložiště a výpočetním, které umožňuje každý zobrazit nezávisle na sobě. SQL datový sklad můžete rychle a jednoduše změnit měřítko na oznámení se nyní přidat další pro využití prostředků. Doplnění to je použití úložiště objektů Blob Azure. Objekty BLOB poskytovat nejen stabilní a replikovanou úložiště, ale taky infrastruktury snadné rozšíření platbou nízké. Pomocí této kombinace měřítko cloudové úložiště a Azure výpočetním SQL datový sklad vám umožní zaplatit výkonu dotazu a úložiště kdykoliv ho budete potřebovat. Změna počtu výpočetním je jednoduchá – stačí přesunutí posuvníkem v portálu Azure doleva nebo doprava, nebo je také možné naplánovat pomocí T-SQL a Powershellu.

Spolu s možnost plně řídit množství výpočetním nezávisle na úložiště SQL datový sklad vám umožní plně pozastavit datový sklad, což znamená, že výpočetním není zaplatit, když ji nepotřebujete. Zachováním úložišti na místě všechny výpočetním vydání do fondu hlavní Azure, ušetříte peníze. V případě potřeby, jednoduše životopis jej mít vaše data a výpočet umožňující vaše pracovní zátěž.

## <a name="data-warehouse-units"></a>Data skladové jednotky

Přidělení zdrojů na SQL Data Warehouse měří se v dat skladové jednotky (DWUs). DWUs jsou měření podkladové zdrojů, jako jsou procesoru, paměti, procesorů, které jsou přidělit datový sklad SQL. Zvýšení počtu DWUs zvýšení výkonu a materiály. Konkrétně DWUs zajistit, že:

- Je možné pro snadnou datový sklad, bez obav o podkladová hardware a software.

- Zlepšení výkonu pro úroveň DWU můžete předpovídání před změnou velikosti datový sklad.

- Základní hardware a software instanci aplikace můžete změnit nebo přesunout beze změny výkon zátěží na projektu.

- Microsoft můžete provést úpravy základní architekturu služby beze změny výkonu svého pracovního vytížení.

- Microsoft rychle dosáhnout zvýšení výkonu v SQL datový sklad tak, že je scalable a rovnoměrně efekty systému.

Data skladové jednotky poskytuje určitý stupeň tři přesné metriky, jež jsou velmi s daty skladování pracovní zátěž výkonu. Cílem je, že následující klíče pracovní zátěž metriky bude lineárně s DWUs, které jste zvolili pro datový sklad.

**Naskenovaný obrázek/agregace:** Tento pracovní zátěž míru trvá standardní datové skladování dotaz, který prohledává velkého počtu řádků a pak provede složité agregace. Toto je vstupu a výstupu a procesoru náročné operace.

**Zatížení:** Tento míru opatření možnost jedí data do služby. Načtení se provádí s PolyBase načítání zástupce k sadám dat z úložiště objektů Blob Azure. Tento míru slouží k zatížení sítě a hlediska procesoru služby.

**Vytvořit tabulku jako vyberte (CTAS):** CTAS opatření možnost Kopírovat tabulku. Tento postup představuje čtení dat z úložiště, distribuce mezi uzly zařízení a znova psaní k základnímu úložišti. Je náročné operace vytížení procesoru, vstupu a výstupu a sítě.

## <a name="pause-and-scale-on-demand"></a>Pozastavit a měřítko jako služba

Pokud potřebujete rychlejší výsledky, zvětšit vaší DWUs a zaplatit vyšší výkon. Když je to potřeba menší výpočet power, zmenšit vaší DWUs a platí pouze pro co byste měli. Si myslíte o změně DWUs v těchto případech:

- Kdy není potřeba spouštění dotazů, například v večerů nebo víkendy, uvedení do nečinnosti dotazů. Kliknutím na pozastavte zdroje pro využití Chcete-li předejít platební DWUs při je nepotřebujete.

- Po systému požadavků málo, zvažte možnost snížení DWU malou velikost. Pořád přístup k datům, ale na úspora nákladů.

- Při provádění operace hodně dat načtení nebo transformaci, je vhodné škálování tak, že vaše data k dispozici rychleji.

Pokud chcete zjistit, je ideální hodnotu DWU, zkuste měřítka nahoru a dolů a spouštění dotazů několik po načtení dat. Protože měřítka je rychlý, můžete zkusit počet různých úrovní výkonu za hodinu nebo nižší.  Mějte na paměti, že SQL datový sklad slouží ke zpracování velkých objemů dat a zobrazíte jeho true možnosti pro změnu velikosti, zejména u větších měřítka, které nabízíme, budete chtít používat velké sadu dat, která přiblíží nebo větší než 1 TB.


## <a name="built-on-sql-server"></a>Založený na serveru SQL Server

SQL datový sklad je založená na modul relační databáze SQL serveru a obsahuje mnoho funkcí, u kterých čekáte od datový sklad organizace. Pokud znáte T-SQL, není těžké si přenést své znalosti SQL datový sklad. Zda jsou rozšířené nebo právě začínáte pracovat, příklady různých dokumentaci vám pomůže začít. Celkově začnete přemýšlet o tak, že jsme jste prvků jazyka SQL datový sklad vytvořen následujícím způsobem:

- SQL datový sklad syntaxí T-SQL pro mnoho operace. Podporuje také celou řadu konstrukce tradiční SQL, jako jsou uložené procedury funkcí definovaných uživatelem, vytvoření oddílů tabulky, indexy a řazení.

- SQL datový sklad také obsahuje mnoho funkcí novější SQL Server, včetně: Skupinový **columnstore** indexy, PolyBase integrace a dat auditování (naplněný souhrnnými hodnocení rizika).

- Některé prvky jazyka T-SQL, které jsou méně běžné pro data skladování úloh nebo novější pro SQL Server nemusí být momentálně neexistuje. Další informace najdete v tématu [migrace si přečtěte následující dokumentaci][].

S jazyce Transact-SQL a funkce prvky mezi SQL serveru, SQL datový sklad, databáze SQL a technologie pro analýzu platformu systému můžete vyvíjet řešení, které odpovídá vašim potřebám data. Můžete rozhodnutí, kam chcete-li zachovat data, na základě výkon, zabezpečení a měřítko požadavky a pak ho přepojit data podle potřeby mezi různými systémy.

## <a name="data-protection"></a>Ochrana dat

SQL datový sklad ukládá všechna data v Azure Premium místně nadbytečné úložiště. Více synchronní kopií data jsou zachovány v centru místních dat zajistit ochranu průhledné dat v případě lokalizované k chybám. Kromě toho SQL datový sklad automaticky vytvoří zálohu aktivní (zrušením pozastaveném) databáze v pravidelných intervalech použití Azure úložiště snímků. Další informace o tom, zálohování a obnovení funguje najdete v tématu [Přehled zálohování a obnovení][].

## <a name="integrated-with-microsoft-tools"></a>Integrovaný s nástroje systému Microsoft

SQL datový sklad také integruje řadu nástrojů, které uživatelé mohou být ničím novým a SQL Server. Jedná se o:

**Nástroje tradiční SQL Server:** SQL datový sklad je plně integrovaný s SQL Server Analysis Services, Integration Services a služby Reporting Services.

**Cloudové nástroje:** SQL datový sklad se dají použít společně s řadou nových nástrojů v Azure, včetně Data Factory, technologie pro analýzu toku, výukové počítače a Power BI. Podrobnější seznam najdete v tématu [Přehled integrovaných nástrojů][].

**Nástrojů jiných výrobců:** Velké množství poskytovatelů nástrojů jiných výrobců certifikaci integrace jejich nástrojů s SQL datový sklad. Úplný seznam najdete v článku [řešení partnerů SQL datový sklad][].

## <a name="hybrid-data-sources-scenarios"></a>Hybridní scénáře zdrojů dat

Pomocí SQL datový sklad PolyBase umožňuje uživatelům špičkovým jádrem přesun dat mezi jejich ekosystému odemknutí umožňuje nastavit upřesňující hybridní scénáře s-relační a místním zdrojům dat.

Polybase umožňuje využívat data z různých zdrojů pomocí známých příkazů T-SQL. Polybase umožňuje dotazu-relačních dat uložených v úložišti objektů Blob Azure jako kdyby normální tabulka. Použití Polybase dotaz není relačních dat a import-relačních dat do SQL datový sklad.

- PolyBase používá externí tabulky přístupu jiných relačních dat. Definice tabulky jsou uloženy v SQL datový sklad, dostanete se k nim pomocí příkazů SQL a nástroje, jak byste měli přístup k normální relačních dat.

- Polybase je která v jeho integrace. Poskytuje stejné funkce a funkce pro všechny zdroje, které podporuje. Dat přečíst Polybase může být v celé řadě různých formátů, včetně soubory s oddělovači nebo ORC soubory.

- PolyBase lze použít pro přístup k úložišti objektů blob, který taky používá jako úložiště pro HDInsight obrázku. To vám umožňuje přístup k stejných dat pomocí nástrojů pro relační a -relační.

## <a name="sla"></a>SLA

SQL datový sklad nabízí produktu úrovně smlouvy o úrovni služeb (SLA) jako součást Microsoft Online Services SLA. Další informace Navštěvujte blog o [SLA SQL datový sklad][]. SLA informace o všechny další produkty můžete navštívit Azure [Smlouvy o úrovni služeb] stránek nebo stáhněte si je na stránce [Multilicenčního programu][] . 

## <a name="next-steps"></a>Další kroky

Teď byste vědět o něco SQL datový sklad, zjistěte, jak rychle [vytvořit datový sklad SQL][] a [načtení ukázkových dat][]. Pokud začínáte Azure, vám můžou přijít vhod [Azure Glosář][] jako narazíte na nové terminologie. Nebo se podívejte se na některé z těchto dalších zdrojů sklad SQL Data.  

- [Příběhy o úspěchu zákazníků]
- [Blogy]
- [Žádost o funkci]
- [Videa]
- [Blogy poradní týmu zákazníka]
- [Vytvoření požadavek podpory můžete]
- [Fórum MSDN]
- [Fórum komunity přetečení zásobníku]
- [Twitter]


<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Vytvoření požadavek podpory můžete]: ./sql-data-warehouse-get-started-create-support-ticket.md
[načtení ukázkových dat]: ./sql-data-warehouse-load-sample-databases.md
[vytvořit datový sklad SQL]: ./sql-data-warehouse-get-started-provision.md
[Přečtěte následující dokumentaci pro migraci]: ./sql-data-warehouse-overview-migrate.md
[Řešení partnerů SQL datový sklad]: ./sql-data-warehouse-partner-business-intelligence.md
[Integrované nástroje – Přehled]: ./sql-data-warehouse-overview-integrate.md
[Zálohování a obnovení základní informace]: ./sql-data-warehouse-restore-database-overview.md
[Glosář Azure]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Příběhy o úspěchu zákazníků]: https://customers.microsoft.com/search?sq=&ff=story_products_services%26%3EAzure%2FAzure%2FAzure%20SQL%20Data%20Warehouse%26%26story_product_families%26%3EAzure%2FAzure%26%26story_product_categories%26%3EAzure&p=0
[Blogy]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Blogy poradní týmu zákazníka]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Žádost o funkci]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Fórum MSDN]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Fórum komunity přetečení zásobníku]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videa]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[SLA pro datový sklad SQL]: https://azure.microsoft.com/en-us/support/legal/sla/sql-data-warehouse/v1_0/
[Multilicenční programy]: http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Smlouvy o úrovni služeb]: https://azure.microsoft.com/en-us/support/legal/sla/
