<properties
   pageTitle="Rozdělením a distributed možnosti tabulky v datových paralelní zpracování (MPP) systémech SQL datový sklad a Parallel Data Warehouse | Microsoft Azure"
   description="Zjistěte, jak distribuci dat pro datových paralelní zpracování (MPP) a možnosti distribuce tabulek v Azure SQL datový sklad a Parallel Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/10/2016"
   ms.author="barbkess"/>


# <a name="distributed-data-and-distributed-tables-for-massively-parallel-processing-mpp"></a>Rozložení dat a distribuované tabulky pro datových paralelní zpracování (MPP)

Zjistěte, jak uživatelská data distribuuje do provincie v Azure SQL datový sklad a Parallel Data Warehouse, které jsou systémy datových paralelní zpracování (MPP) společnosti Microsoft. Navrhování datový sklad efektivně používat rozložení dat vám pomůže při dosažení zpracování výhody architektury MPP dotazů. Několik možností návrh databáze může mít významný vliv na zvýšení výkonu dotazu.  

>[AZURE.NOTE] Azure SQL datový sklad a Parallel Data Warehouse používat stejný návrh datových paralelní zpracování (MPP), ale mají několik rozdílů kvůli základní platformě. SQL datový sklad je platforma jako služba (PaaS), která poběží v Azure. Paralelní datový sklad běží na analýzy platformu systému Podporujících což je místní spotřebiče, které se spouští v systému Windows Server.

## <a name="what-is-distributed-data"></a>Co je rozložení dat?

Rozložení dat v SQL datový sklad a Parallel Data Warehouse odkazuje uživatelská data, která se ukládá do několika umístění systému MPP. Každé z těchto umístění funguje jako nezávisle na úložiště a zpracování jednotku, která běží dotazy na jeho část data. Distribuované data jsou zásadní spouštění dotazů paralelně dosáhnout výkonu vysoké dotazů.

K distribuci dat, přiřadí datový sklad každý řádek tabulky uživatele do jednoho distribuované umístění.  Můžete distribuovat tabulky se metodu hash rozdělení nebo kruhového metody. Způsob rozdělení podle příkaz CREATE TABLE. 

## <a name="hash-distributed-tables"></a>Distribuované hash tabulek
  
Následující obrázek znázorňuje, jak plné (-distributed tabulka) ukládají jako tabulku hash distributed. Deterministický funkce přiřadí každý řádek patří k jedné rozdělení. Definice tabulky jeden ze sloupců označen jako rozdělení sloupce. Funkce hash použije hodnotu ve sloupci rozdělení přiřadit každý řádek rozdělení.

V úvahu výkonu pro výběr rozdělení sloupce, třeba odlišnosti, zkosení dat a uvedených typech dotazů spustit v počítači.
  
![Distribuované tabulky] (media/sql-data-warehouse-distributed-data/hash-distributed-table.png "Distribuované tabulky")  
  
-   Každý řádek patří do jednoho rozdělení.  
  
-   Algoritmus hash deterministický přiřadí každý řádek jednu distribuční.  
  
-   Počet řádků tabulky na rozdělení se liší podle různých velikostí tabulky.

## <a name="round-robin-distributed-tables"></a>Kruhového distribuované tabulek

Distribuované tabulky kruhového distribuuje řádky přiřazením každý řádek distribuční sekvenční způsobem. Tento způsob je rychlý k načtení dat do tabulky kruhového, ale dotazu výkon pravděpodobně nižší.  Obvykle spojující tabulky kruhového vyžaduje promísení řádky, které chcete povolit dotazu vytvářejí přesné výsledek, který dlouho.

## <a name="distributed-storage-locations-are-called-distributions"></a>Distribuované úložišť nazývají rozdělení

Každý distribuované umístění se nazývá rozdělení. Při spuštění dotazu paralelně každé rozdělení provede dotaz SQL zobrazený na jeho část data. SQL datový sklad používá databáze SQL spusťte dotaz. SQL Server Parallel Data Warehouse používá spusťte dotaz. Tento sdílený není nic architektura návrh je základní k dosažení škálování paralelní výpočty.

### <a name="can-i-view-the-distributions"></a>Můžete zobrazit rozdělení?

Každý distribuční má ID rozdělení a je zobrazena v zobrazeních systému, které se týkají SQL datový sklad a Parallel Data Warehouse. ID rozdělení slouží k řešení potíží s výkonem dotazu a jiným problémům. Seznam systémových zobrazení najdete v článku [zobrazení systémové MPP](sql-data-warehouse-reference-tsql-statements.md).

## <a name="difference-between-a-distribution-and-a-compute-node"></a>Rozdíl mezi rozdělení a výpočetní uzly

Rozdělení je základní jednotka pro ukládání rozložení dat a zpracování paralelní dotazy. Distribuce seskupeny do výpočetního uzlů. Každý výpočetní uzly sleduje jeden nebo více rozdělení.  

-   Technologie pro analýzu platformu systému používá výpočetním uzly jako ústřední prvek hardwarové architektura a škálování možnosti. Vždy používala osmi rozdělení za výpočetní uzly uvedeno v diagramu pro hash distributed tabulky. Počtu uzlů výpočetním a proto počet rozdělení, je určený podle počtu uzlů výpočetním zakoupených pro zařízení. Platí při zakoupení osm uzlů výpočetním, například dostanete 64 rozdělení (8 výpočetní uzly x 8 distribuce/uzly). 

-   SQL datový sklad má pevného počtu 60 distribuce a flexibilní počtu uzlů výpočetním. Uzly výpočetním jsou implementovaná s Azure výpočetních a paměťových zdrojů. Počtu uzlů výpočetním můžete změnit podle předpisů rozhraní služby vytížení back-end a výpočetní kapacitu (DWUs) pro datový sklad zadáte. Pokud se změní počtu uzlů výpočetního počet rozdělení za výpočetní uzly změníte. 

### <a name="can-i-view-the-compute-nodes"></a>Můžete zobrazit uzly výpočetním?

Každý výpočetní uzly má uzel ID a je zobrazena v zobrazeních systému, které se týkají SQL datový sklad a Parallel Data Warehouse.  Zobrazí se výpočetní uzly vyhledáním node_id sloupce v zobrazení systémové jejichž názvy začínají sys.pdw_nodes. Seznam systémových zobrazení najdete v článku [zobrazení systémové MPP](sql-data-warehouse-reference-tsql-statements.md).

## <a name="Replicated"></a>Replikovat tabulky pro – paralelní datový sklad 
  
Platí pro: paralelní datový sklad

Kromě použití distribuované tabulek, Parallel Data Warehouse nabízí možnost replikovat tabulek. *Replikovat tabulky* je tabulka, která je uložena v celém v jednotlivých uzlech výpočetním. Replikace tabulky odebere nutnosti přenášet jeho řádky tabulky mezi uzly výpočetním před použitím tabulky ve spojení nebo agregace. Replikovanou tabulky jsou možné s malé tabulky pouze z důvodu úložiště navíc potřebných pro uložení celou tabulku v jednotlivých uzlech výpočetním.  
  
Na následujícím obrázku vidíte replikovanou tabulku, která je uložena v jednotlivých uzlech výpočetním. Tabulka replikovanou uložena všech disků přiřazená výpočetní uzly. Tato strategie disku implementovaná pomocí skupin souborů systému SQL Server.  
  
![Replikovaná tabulky] (media/sql-data-warehouse-distributed-data/replicated-table.png "Replikovaná tabulky") 
  
## <a name="next-steps"></a>Další kroky
  
Efektivní použití distribuované tabulek, najdete v článku [distribuce tabulek v SQL datový sklad](sql-data-warehouse-tables-distribute.md)  
  



  
