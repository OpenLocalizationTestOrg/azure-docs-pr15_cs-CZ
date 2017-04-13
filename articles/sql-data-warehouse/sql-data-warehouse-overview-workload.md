<properties
   pageTitle="Datový sklad zátěží na projektu"
   description="Pružnost SQL datový sklad můžete zvětšit, zmenšit nebo pozastavení výpočetního výkonu pomocí posuvné škále dat skladové jednotky (DWUs). Tento článek vysvětluje metriky datový sklad a jak se týkají DWUs. "
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
   ms.date="07/25/2016"
   ms.author="barbkess;mausher;jrj;sonyama"/>


# <a name="data-warehouse-workload"></a>Datový sklad zátěží na projektu
Datový sklad úlohu odkazuje na všechny operace, které transpire proti datový sklad. Pracovní zátěž datový sklad zahrnuje celý proces načítání dat do sklad provádění analýzy a vytváření sestav na datový sklad, Správa dat v datový sklad a export dat z datový sklad. Název hloubkové a šířka tyto součásti, jsou často odpovídající úrovně splatnost datový sklad.


## <a name="new-to-data-warehousing"></a>Nové datové sklady?
Datový sklad je sada data, která je načteno z jedné nebo více zdrojů dat a slouží k provádění úlohy funkcí business intelligence například vytváření sestav a analýza dat.

Sklady dat jsou charakteristické tak, že dotazy, které skenování větší čísla řádků, velké oblasti dat a může vrátit relativně velké výsledků pro účely analýzy a vytváření sestav. Sklady dat jsou také které je určeno argumenty zatížení relativně velkých dat a small úrovni transakce vloží/aktualizace a odstraní.

- Při uložení dat tak, aby optimalizuje dotazy, které je potřeba zkontrolovat většího počtu řádků nebo velké oblasti dat datový sklad provede nejlepší. Tento typ prohledávání funguje nejlépe, když data se ukládají a prohledávat podle sloupců, ne řádků.

>[AZURE.NOTE] Index v paměti columnstore používá sloupec úložiště, nabízí až 10 × komprese zisků a 100 x zlepšení výkonu dotazu přes tradiční binární stromy pro vytváření sestav a technologie pro analýzu dotazy. Jsme jako standardní pro ukládání a procházení velkých dat v datový sklad zvažte columnstore indexy.

- Datový sklad má odlišné požadavky než systému, který optimalizuje pro online zpracování transakcí (OLTP). Systém OLTP má mnoho vložit, aktualizovat a odstraňovat operace. Tyto operace snaží konkrétních řádků v tabulce. Tabulka usiluje provést nejlépe, když jsou data uložena způsobem po řádcích. Data lze řadit rychle pomocí dělení a dobytí přístup s názvem binární strom nebo btree vyhledávání.


## <a name="data-loading"></a>Načítání dat
Načítání dat je velká část pracovní zátěž datový sklad. Firmy mají obvykle zaneprázdněné OLTP systému, který ke sledování změn během dne při zákazníci generují transakce business. Pravidelně často v noci době údržby transakce jsou přesouvané nebo kopírované k datový sklad. Jakmile se data v datový sklad, analytici analýza a rozhodování na kartě data.

- Proces načítání obvykle se nazývá ETL extrahovat, transformace a načíst. Obvykle potřeba data transformovat tak, aby byl konzistentní s jiná data v datový sklad. Dříve firmy lze vyhrazené servery ETL provést transformací. Teď pomocí tohoto rychlého datových paralelní zpracování můžete načíst data do SQL datový sklad nejdřív a potom provést transformací. Tento proces se nazývá extrahovat, načíst a transformovat (ELT) a je stane nový standard pracovní zátěž datový sklad.

> [AZURE.NOTE] S SQL serveru 2016, můžete teď provádění analýzy v reálném čase v tabulce aplikace OLTP. To nenahrazuje požadovaný datový sklad ukládat a analyzovat data, ale ho umožňují provádění analýz v reálném čase.

### <a name="reporting-and-analysis-queries"></a>Vytváření sestav a analýzy dotazů
Vytváření sestav a analýzy dotazy se často zařadit do malá, střední a velké podle určitá kritéria, ale obvykle se podle času. Ve většině sklady dat je smíšený pracovní zátěž rychlé spuštění versus-náročné dotazy. V obou případech je důležité určit tato kombinace a určení jeho četnost (každou hodinu, denně konci měsíce, čtvrtletí end a tak dále). Je důležité pochopit, že pracovní zátěž smíšených dotazu svázáno s souběžné, průběžné s velkými kapacitou plánování datový sklad.

- Plánování kapacity může být složitou úlohu pro úlohu smíšených dotazu, zvlášť když potřebujete rozsáhlé čas kapacita přidáte datový sklad. SQL datový sklad odebere naléhavé části plánování kapacity vzhledem k tomu můžete zvětšovat a zmenšovat výpočetním kapacity kdykoli a od úložiště a kapacita výpočetním mají nezávisle na sobě velikost.

### <a name="data-management"></a>Správa dat
Správa dat je důležité, zvlášť když víte, že se může docházet místa na disku nevidět. Sklady dat obvykle rozdělit data smysluplné oblastí, které jsou uložené jako oddíly v tabulce. Všechny produkty založené na SQL Server umožňují přesunete oddíly a odhlášení z tabulky. Tento oddíl přepínání umožňuje přesunout starší data k základnímu úložišti levnější a udržovat aktuální data k dispozici na online úložiště.

- Indexy Columnstore domovské stránce podpory rozdělený tabulek. Pro columnstore indexy rozdělený tabulky jsou použity pro správu dat a archivace. U tabulek uložených řádek po řádku oddíly mít větší role výkonu dotazu.  

- PolyBase přehraje důležitou roli při správě data. Použití PolyBase, máte možnost archivovat starší data k úložišti objektů blob Hadoop nebo Azure.  Protože data je pořád online nabízí spoustu možností.  Může trvat déle k načtení dat z Hadoop, ale kompromis doby načítání může převážit náklady na uložení.

### <a name="exporting-data"></a>Export dat
Jedním ze způsobů být k dispozici pro zprávy a analýzy dat je odešlete data z datový sklad servery vyhrazené pro spuštění sestavy a analýzy. Tyto servery nazývají data marts. Můžete třeba předem zpracování dat sestavy a pak je exportovat z datový sklad mnoho serverů světě ho ho široce zpřístupnit pro zákazníky a analytici.

- Pro generování sestav, každý noční může naplnění serverů jen pro čtení pro vykazování se snímkem denní data. To vám větší šířku pásma pro zákazníky při snížení potřeb zdrojů výpočetním na datový sklad. Z zabezpečení poměr umožňují dat marts snižte počet uživatelů, kteří mají přístup k datový sklad.
- Pro analýzy můžete vytvořit datovou krychli analýzy na datový sklad a dělat analýzu proti datový sklad nebo předem zpracování dat a exportovat na server analysis pro další analýzu.

## <a name="next-steps"></a>Další kroky
Teď byste vědět o něco SQL datový sklad, zjistěte, jak rychle [vytvořit datový sklad SQL][] a [načtení ukázkových dat][].

<!--Image references-->

<!--Article references-->
[načtení ukázkových dat]: ./sql-data-warehouse-load-sample-databases.md
[vytvořit datový sklad SQL]: ./sql-data-warehouse-get-started-provision.md

<!--MSDN references-->

<!--Other web references-->
