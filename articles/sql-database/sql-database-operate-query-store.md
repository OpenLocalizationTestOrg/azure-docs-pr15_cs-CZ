<properties
   pageTitle="Provozní úložiště dotazu v databázi Azure SQL"
   description="Naučte se pracovat úložišti dotazu v databázi SQL Azure"
   keywords=""
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="sqldb-performance"
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="carlrab"/>

# <a name="operating-the-query-store-in-azure-sql-database"></a>Provozní úložišti dotazu v databázi Azure SQL 

Úložiště dotazu v Azure je funkce plně spravovaných databáze, která nepřetržitě shromažďuje a prezentuje indexy historické podrobné informace o všech dotazů. Úložiště dotazu můžete myslete představit podobně jako letadla ve letu dat nástroj pro zaznamenávání, která výrazně zjednodušuje výkonu dotazu Poradce při potížích pro cloudu a místní zákazníci. Tento článek vysvětluje zvláštní aspekty provozní úložiště dotazu v Azure. Použití tato data předem shromážděných dotazu, můžete rychle Diagnostika a vyřešit problémy s výkonem a tím vám další zaměřené na své firmě. 

Dotaz úložiště byl [globálně dostupné](https://azure.microsoft.com/updates/general-availability-azure-sql-database-query-store/) v databázi SQL Azure od listopadu 2015. Dotaz úložiště je základem analýzy výkonu a optimalizace funkce, jako jsou [Advisor databáze SQL a výkonu řídicího panelu](https://azure.microsoft.com/updates/sqldatabaseadvisorga/). V okamžiku, kdy publikování v tomto článku úložiště dotazu běží více než 200 000 uživatelských databází v Azure, shromažďování informace související s dotazy několik měsíců nepřetržitě.

> [AZURE.IMPORTANT] Společnost Microsoft se aktivaci úložiště dotazu pro všechny databáze Azure SQL (stávající i nový). 

## <a name="optimal-query-store-configuration"></a>Konfigurace optimální úložiště dotazu

Tato část popisuje výchozí hodnoty optimální konfigurace, které slouží k zajištění spolehlivé operace dotazu Store a Google závislá funkce, jako jsou [Advisor databáze SQL a výkonu řídicího panelu](https://azure.microsoft.com/updates/sqldatabaseadvisorga/). Výchozí konfigurace je optimalizována pro kolekci souvislá data, která je minimální čas strávený vypnuto/READ_ONLY států.

| Konfigurace | Popis | Výchozí | Komentář |
| ------------- | ----------- | ------- | ------- |
| MAX_STORAGE_SIZE_MB | Určuje limit úložiště dotazu může trvat uvnitř z databáze zákazníků místo na data | 100 | Vynucení pro nové databáze |
| INTERVAL_LENGTH_MINUTES | Definuje velikost časového intervalu při které statistiky shromážděných runtime pro plány dotazu jsou agregované a zachován. Každý plán aktivní dotaz má maximálně jeden řádek pro určité době tato pole definovány této konfigurace | 60   | Vynucení pro nové databáze |
| STALE_QUERY_THRESHOLD_DAYS | Založená na čase vyčištění zásad, které řídí období uchování trvalých runtime Statistika a neaktivní dotazů | 30 | Vynucení nové databáze i s předchozí výchozí (367) |
| SIZE_BASED_CLEANUP_MODE | Určuje, zda vyčištění automatické dat provede po velikost datový úložiště dotazu blíží limit | AUTOMATICKÉ | Vynucení pro všechny databáze |
| QUERY_CAPTURE_MODE | Určuje, zda jsou sledovány všechny dotazy nebo jen podmnožinu dotazů | AUTOMATICKÉ | Vynucení pro všechny databáze |
| FLUSH_INTERVAL_SECONDS | Určuje maximální dobu, které nezaznamenávají runtime statistiky jsou uloženy v paměti, před použitím Vyčištění disku | 900 | Vynucení pro nové databáze |
||||||

> [AZURE.IMPORTANT] Tyto výchozí hodnoty automaticky, použijí se při vyplňování konečný aktivace úložiště dotazu v všechny databáze Azure SQL (viz před důležité poznámky). Po tomto light nahoru databáze SQL Azure nebude měnit hodnoty konfiguraci nastavil zákazníky, pokud negativně ovlivnit primární pracovního vytížení a spolehlivé operace úložišti dotazu.

Pokud chcete mít s vlastními nastaveními, pomocí [Vlastnosti databáze s možnostmi dotazu úložiště](https://msdn.microsoft.com/library/bb522682.aspx) obnovit konfigurace předchozího stavu. Podívejte se na [Osvědčené postupy se obchodu dotazu](https://msdn.microsoft.com/library/mt604821.aspx) Pokud chcete zjistit, jak horní rozhodli, že parametry optimální konfigurace.

## <a name="next-steps"></a>Další kroky

[Přehled výkonu databáze SQL](sql-database-performance.md)

## <a name="additional-resources"></a>Další zdroje informací

Pro další informace o rezervaci v následujících článcích:

- [Záznam dat letu databáze](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database) 

- [Sledování výkonu pomocí úložišti dotazu](https://msdn.microsoft.com/library/dn817826.aspx)

- [Příklady použití úložiště dotazu](https://msdn.microsoft.com/library/mt614796.aspx)

- [Sledování výkonu pomocí úložišti dotazu](https://msdn.microsoft.com/library/dn817826.aspx) 
