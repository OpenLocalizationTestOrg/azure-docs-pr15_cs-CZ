<properties
   pageTitle="Správa výpočetního výkonu v Azure SQL datový sklad (přehled) | Microsoft Azure"
   description="Výkon měřítko, funkce v Azure SQL datový sklad. Rozšiřování úpravou DWUs nebo pozastavit a obnovit výpočetním zdroje snížit náklady."
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
   ms.date="09/03/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-overview"></a>Správa výpočetního výkonu v Azure SQL datový sklad (přehled)

> [AZURE.SELECTOR]
- [Základní informace](sql-data-warehouse-manage-compute-overview.md)
- [Portál](sql-data-warehouse-manage-compute-portal.md)
- [Prostředí PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [ZBÝVAJÍCÍ](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)

Architektura SQL datový sklad odděluje úložiště a výpočetním umožňující každé zobrazit nezávisle na sobě. V důsledku toho rozšiřování výkon při ukládání náklady zaplacením pouze výkon kdykoliv ho budete potřebovat. 

Přehled popisuje následující možnosti škálování výkonu SQL datový sklad a uvádí doporučení ohledně toho, jak a jejich použití. 

- Měřítko výpočet power úpravou [dat skladové jednotky (DWUs)][]
- Pozastavení a opakované využití prostředků

<a name="scale-performance-bk"></a>

## <a name="scale-performance"></a>Měřítko výkonu

V SQL datový sklad můžete rychle změnit velikost výkonu, nebo zpět zvětšení nebo zmenšení výpočetním zdrojů procesoru, paměti a šířky pásma vstupu a výstupu. Zobrazit výkonu, je třeba udělat stačí upravte počet [dat skladové jednotky (DWUs)][] , která SQL datový sklad přidělí do vaší databáze. SQL datový sklad rychle provede změny a úchyty pro všechny základní změny hardware a software.

Potřebujete-li prozkoumat, jaký druh procesorů, kolik paměti nebo jaký druh úložiště potřebujete vysoký výkon v datový sklad pryč jsou dny. Vložením datový sklad v cloudu už máte řešit problémy nízkoúrovňový hardwaru. Místo toho SQL datový sklad zeptá tuto otázku: jak rychle chcete analyzovat data? 

### <a name="how-do-i-scale-performance"></a>Jak můžu změnit velikost výkon?

Elastically zvětšit nebo zmenšit výpočetního výkonu, jednoduše změňte [data skladové jednotky (DWUs)][] nastavení databáze. Výkon zvýší lineárně přidat další DWU.  Na vyšší úrovni DWU budete muset přidat více než 100 DWUs setkat významné zvýšení výkonu. Aby se v DWUs vyberte smysluplné přeskakování, nabízíme DWU úrovně, které vám umožní dosažení nejlepších výsledků.
 
Chcete-li upravit DWUs, můžete použít některou z těchto metod jednotlivé.

- [Měřítko výpočet power portálem Azure][]
- [Měřítko výpočet power pomocí prostředí PowerShell][]
- [Měřítko výpočetního výkonu s rozhraní REST API][]
- [Měřítko výpočet power s TSQL][]

### <a name="how-many-dwus-should-i-use"></a>Kolik DWUs mám použít?
 
Výkon SQL datový sklad přizpůsobí lineárně a změňte z jednoho výpočetním měřítko do druhého (například z 100 DWUs k 2000 DWUs) se stane v sekundách. To vám umožní experimentovat s různým nastavením DWU až určíte, váš scénář Nejlepší přizpůsobení.

Pokud chcete zjistit, je ideální hodnotu DWU, zkuste měřítka nahoru a dolů a spouštění dotazů několik po načtení dat. Protože měřítka je rychlý, můžete zkusit počet různých úrovní výkonu za hodinu nebo nižší. Mějte na paměti, že SQL datový sklad slouží ke zpracování velkých objemů dat a zobrazíte jeho true možnosti pro změnu velikosti, zejména u větších měřítka, které nabízíme, budete chtít používat velké sadu dat, která přiblíží nebo větší než 1 TB.

Doporučení pro vyhledání nejlepší DWU pro vaše pracovní zátěž:

1. Datový sklad ve vývoji začněte tak, že vyberete malým počtem poštovních DWUs.  Vhodná výchozí hodnota je DW400 nebo DW200.
2. Sledujte výkon aplikace počtu DWUs vybraná ve srovnání s výkonem, které se můžou přihlížet.
3. Zjistit, kolik rychleji nebo pomaleji výkonu by měl být za vás k dosažení optimálního výkonu úrovně pro vašim požadavkům tak, že za předpokladu, že lineární měřítko.
4. Zvětšit nebo zmenšit počet DWUs podle jak mnohem rychleji nebo pomaleji má vaše pracovní zátěž provádět. Služba rychle odpovědět a upravte zdroje výpočetním požadavkům na nové DWU.
5. Pokračujte v provádění úprav, dokud nebude aktivován úroveň optimální výkon pro vašim požadavkům pro firmy.

### <a name="when-should-i-scale-dwus"></a>Kdy by měla měřítko DWUs?

Pokud potřebujete rychlejší výsledky, zvětšit vaší DWUs a zaplatit vyšší výkon.  Když je to potřeba menší výpočet power, zmenšit vaší DWUs a platí pouze pro co byste měli. 

Doporučení pro kdy zobrazit DWUs:

1. Pokud aplikace má kolísání pracovní zátěž škála DWU vyrovnání nahoru nebo dolů a přizpůsobí vrcholy a nízké body. Například pokud váš pracovní zátěž obvykle nejlepší při Konec měsíce, plánování přidávat další DWUs během tyto dny, a pak Neomezovat po období Špička se.
2. Před provedení operace načtení nebo transformace hodně dat škálování DWUs tak, že vaše data k dispozici rychleji.

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Pozastavit výpočetním

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Pozastavit databáze, vyberte některou z těchto jednotlivé postupů.

- [Pozastavit výpočetním portálem Azure][]
- [Pozastavit výpočetním pomocí prostředí PowerShell][]
- [Pozastavit výpočetním s rozhraní REST API][]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Pro využití životopisu

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Obnovit databázi, vyberte některou z těchto jednotlivé postupů.

- [Životopis výpočetním portálem Azure][]
- [Životopis výpočetním pomocí prostředí PowerShell][]
- [Životopis výpočetním s rozhraní REST API][]

## <a name="permissions"></a>Oprávnění

Změna velikosti databáze budou vyžadovat oprávnění popsaná v [Vlastnosti databáze][].  Pozastavení a obnovení budou vyžadovat oprávnění [Přispěvatel databáze SQL][] , konkrétně Microsoft.Sql/servers/databases/action.

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Další kroky
Získáte následující články vám umožňují principech některé další klíčové výkon:

- [Správa pracovního vytížení a souběžné][]
- [Přehled návrhu tabulky][]
- [Distribuce tabulky][]
- [Indexování tabulky][]
- [Vytvoření oddílů tabulky][]
- [Statistiky tabulek][]
- [Doporučené postupy][]

<!--Image reference-->

<!--Article references-->
[data skladové jednotky (DWUs)]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units

[Měřítko výpočet power portálem Azure]: ./sql-data-warehouse-manage-compute-portal.md#scale-compute-bk
[Měřítko výpočet power pomocí prostředí PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#scale-compute-bk
[Měřítko výpočetního výkonu s rozhraní REST API]: ./sql-data-warehouse-manage-compute-rest-api.md#scale-compute-bk
[Měřítko výpočet power s TSQL]: ./sql-data-warehouse-manage-compute-tsql.md#scale-compute-bk

[capacity limits]: ./sql-data-warehouse-service-capacity-limits.md

[Pozastavit výpočetním portálem Azure]:  ./sql-data-warehouse-manage-compute-portal.md#pause-compute-bk
[Pozastavit výpočetním pomocí prostředí PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#pause-compute-bk
[Pozastavit výpočetním s rozhraní REST API]: ./sql-data-warehouse-manage-compute-rest-api.md#pause-compute-bk

[Životopis výpočetním portálem Azure]:  ./sql-data-warehouse-manage-compute-portal.md#resume-compute-bk
[Životopis výpočetním pomocí prostředí PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#resume-compute-bk
[Životopis výpočetním s rozhraní REST API]: ./sql-data-warehouse-manage-compute-rest-api.md#resume-compute-bk

[Správa pracovního vytížení a souběžné]: ./sql-data-warehouse-develop-concurrency.md
[Přehled návrhu tabulky]: ./sql-data-warehouse-tables-overview.md
[Distribuce tabulky]: ./sql-data-warehouse-tables-distribute.md
[Indexování tabulky]: ./sql-data-warehouse-tables-index.md
[Vytvoření oddílů tabulky]: ./sql-data-warehouse-tables-partition.md
[Statistiky tabulek]: ./sql-data-warehouse-tables-statistics.md
[Doporučené postupy]: ./sql-data-warehouse-best-practices.md 
[development overview]: ./sql-data-warehouse-overview-develop.md

[Přispěvatel databáze SQL]: ../active-directory/role-based-access-built-in-roles.md#sql-db-contributor

<!--MSDN references-->
[PŘÍKAZ ALTER DATABÁZE]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[Azure portal]: http://portal.azure.com/
