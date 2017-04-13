<properties 
   pageTitle="Přehled výkonu databáze Azure SQL | Microsoft Azure" 
   description="Databáze SQL Azure obsahuje nástroje pro sledování výkonu vám pomůže identifikovat oblasti, které můžete zlepšit výkon aktuální dotazu." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="07/19/2016"
   ms.author="sstein"/>

# <a name="sql-database-performance-insight"></a>Přehled výkonu databáze SQL

Databáze SQL Azure obsahuje nástroje pro sledování výkonu vám pomůže identifikovat a vylepšení výkonu databáze pomocí inteligentní ladění akce a doporučení. 

1. Přejděte do databáze na [Portál Azure](http://portal.azure.com) a klikněte na **všechna nastavení** > **výkonu **  >  **– Přehled** a otevře se stránka **výkonu** . 


2. Klikněte na **doporučení** otevřete [Advisor SQL databáze](#sql-database-advisor)a klikněte na **dotazy** otevřete [Přehled výkonu dotazu](#query-performance-insight).

    ![Zobrazení výkonu](./media/sql-database-performance/entries.png)



## <a name="performance-overview"></a>Přehled sledování výkonu

Kliknutím na **Přehled** nebo na dlaždici **výkonu** přejdete na výkon řídicí panel pro databázi. Toto zobrazení obsahuje souhrn výkon databáze a pomůže vám s výkonem optimalizace a řešení potíží. 

![Výkon](./media/sql-database-performance/performance.png)

- Dlaždice **doporučení** poskytuje přehled optimalizace doporučení pro databázi (horní 3 doporučení jsou zobrazeny Pokud je potřeba udělat další). Klepnutím na této dlaždici přejdete k **SQL databázi Advisor**. 
- Dlaždice **Optimalizace aktivity** obsahuje souhrn probíhající a dokončené optimalizace akce pro databázi, která nabízí rychlý přehled do historie optimalizace aktivity. Kliknutím na této dlaždici přejdete na úplné optimalizace zobrazení historie databáze.
- Dlaždice **Automatické ladění** zobrazuje optimalizace automatické konfigurace databáze (ladění akcí, které jsou nakonfigurované automaticky u databáze). Kliknutím na této dlaždici se otevře dialogové okno automatické konfigurace.
- Dlaždice **databázových dotazů** zobrazuje souhrn výkonu dotazu pro databázi (celková DTU použití a nahoře na prostředky dotazů). Kliknutím na této dlaždici přejdete na **Přehled výkonu dotazu**.



## <a name="sql-database-advisor"></a>Konference Advisor databáze SQL


[Konference Advisor databáze SQL](sql-database-advisor.md) doporučí inteligentní ladění, které můžete zlepšit výkonu vaší databáze. 

- Doporučení ohledně toho, které indexy vytvořit nebo přetáhnout (a možnost použít index doporučení automaticky bez zásahu uživatele a automaticky vrácení zpět doporučení, která mít negativní vliv na výkon).
- Doporučení pro problémy schématu jsou určeny v databázi.
- Doporučení pro využívat dotazů s parametry dotazů.




## <a name="query-performance-insight"></a>Přehled výkonu dotazu

[Přehled výkonu dotazu](sql-database-query-performance.md) umožňuje méně času řešení potíží s výkonem databáze poskytnutím:

- Podrobnější využití prostředků (DTU) databáze. 
- Začátek procesoru jinými dotazy, které můžete potenciálně optimalizovaných pro zvýšení výkonu. 
- Umožňuje přecházet na podrobnější informace dotazu. 


## <a name="additional-resources"></a>Další zdroje informací

- [Azure SQL databáze výkonu pokyny pro jeden databáze](sql-database-performance-guidance.md)
- [Pokud má být použita fondu pružná databáze?](sql-database-elastic-pool-guidance.md)