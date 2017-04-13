<properties
    pageTitle="Sledovat XTP v paměti úložiště | Microsoft Azure"
    description="Sledování XTP v paměti úložiště a odhadu použít, kapacity; řešení chyby kapacita 41823"
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="jodebrui"/>


# <a name="monitor-in-memory-oltp-storage"></a>Sledování OLTP v paměti úložiště

Pokud chcete použít [v paměti OLTP](sql-database-in-memory.md), data ve paměť optimalizována tabulek a proměnných tabulky uložena v OLTP v paměti úložiště. Jednotlivé vrstvy služeb Premium obsahuje maximální velikosti úložiště OLTP v paměti, které jsou uvedeny v [článku – úrovně služeb SQL databáze](sql-database-service-tiers.md#service-tiers-for-single-databases). Až toto omezení je překročeno, vkládat a aktualizovat operace může spustit (došlo k chybě 41823). V tomto okamžiku bude nutné buď odstranit data, která chcete uvolnit paměti nebo upgradovat vrstvu výkonu databáze.

## <a name="determine-whether-data-will-fit-within-the-in-memory-storage-cap"></a>Zjistit, jestli data se vejde do úložiště v paměti zakončení

Určení zakončení úložiště: naleznete v [článku SQL databáze služby úrovní](sql-database-service-tiers.md#service-tiers-for-single-databases) úložiště Caps (čepice) z různých úrovní služby Premium.

Odhad požadavky na paměť paměť optimalizována tabulky funguje stejným způsobem jako pro systém SQL Server tak, jak se má v databázi SQL Azure. Zkontrolujte toto téma na [webu MSDN](https://msdn.microsoft.com/library/dn282389.aspx)chvíli trvat.

Všimněte si, že tabulky a proměnných řádky tabulky, stejně jako indexy počítat směrem k velikost dat maximální uživatele. Kromě toho ALTER TABLE musí dostatek místa pro vytvoření nové verze celou tabulku a jeho indexy.

## <a name="monitoring-and-alerting"></a>Sledování a výstrahy.

Můžete sledovat v paměti úložiště použití jako procentuální hodnotu [úložiště Caps výkonu osy](sql-database-service-tiers.md#service-tiers-for-single-databases) Azure [portálu](https://portal.azure.com/): 

- Na zásuvné databáze vyhledejte pole využití prostředků a klikněte na možnost upravit.
- Vyberte metriky `In-Memory OLTP Storage percentage`.
- Přidat upozornění, klikněte na pole využití prostředků a otevřete metrických zásuvné a potom klikněte na Přidat upozornění.

Nebo umožňuje znázornit využití úložiště v paměti následující dotaz:

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-memory-situations---error-41823"></a>Oprava nedostatku paměti situacích - chyby 41823

Spuštění nedostatku paměti výsledky v tématu vložení, aktualizovat a vytvořte operace neúspěšně s chybovou zprávou 41823.

Chybová zpráva 41823 označuje, že paměť optimalizována tabulek a proměnných tabulky překročily maximální velikost.

Tuto chybu můžete vyřešit, buď:


- Odstranit data z tabulek paměť optimalizována potenciálně odstranění data, která chcete tradiční diskové tabulek; nebo
- Upgradujte vrstvy služeb jedna z nich dost v paměti úložiště pro data, která je potřeba mít na paměti optimalizované tabulek.

## <a name="next-steps"></a>Další kroky
Další zdroje informací o používání [Sledování databáze SQL Azure Správa dynamických zobrazení](sql-database-monitoring-with-dmvs.md)
