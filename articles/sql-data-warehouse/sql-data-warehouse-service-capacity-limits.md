<properties
   pageTitle="Omezení kapacity SQL datový sklad | Microsoft Azure"
   description="Maximální hodnoty u připojení, databází, tabulky nebo dotazy na SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/01/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="sql-data-warehouse-capacity-limits"></a>Omezení SQL datový sklad kapacity

V následujících tabulkách obsahují maximální hodnoty pro jednotlivé součásti webové části datový sklad SQL Azure povolené.


## <a name="workload-management"></a>Správa zátěží na projektu

| Kategorie            | Popis                                       | Maximální            |
| :------------------ | :------------------------------------------------ | :----------------- |
| [Data skladové jednotky (DWU)][]| Max DWU pro jeden datový sklad SQL | 6000               |
| [Data skladové jednotky (DWU)][]| Max DWU jednoho SQL serveru         | 6000 ve výchozím nastavení<br/><br/> Ve výchozím nastavení obsahuje každý SQL server (například myserver.database.windows.net) kvóty DTU 45,000, které umožňuje až 6000 DWU. Tento kvóta je jednoduše limit zabezpečení. Můžete zvýšit kvóty [vytváření požadavek podpory můžete][] a výběrem *kvóty* psaní žádost.  Chcete-li vypočítat vaší DTU musí, násobit 7.5 podle součtu, třeba DWU. Vaše aktuální DTU spotřebu zásuvné SQL serveru můžete zobrazit na portálu. Pozastavené a rušení pozastaveném databází počítání směrem k DTU kvóty. |
| Připojení k databázi | Souběžné otevřených relací                          | 1 024<br/><br/>Podporujeme maximálně 1 024 aktivní připojení, z nichž každá odesílat žádosti o k databázi SQL datový sklad ve stejnou dobu. Všimněte si, že jsou omezení počtu dotazy, které skutečně můžete současně provést. Při překročení limitu souběžné žádost přejde do vnitřní frontě kde čeká na zpracování.|
| Připojení k databázi | Maximální velikost paměti pro připravené příkazy            | 20 MB              |
| [Správa zátěží na projektu][] | Maximální souběžné dotazů                    | 32<br/><br/> Ve výchozím nastavení můžete provést SQL datový sklad maximálně 32 souběžné dotazů a fronty zbývající dotazy.<br/><br/>Úroveň souběžné může snížit, pokud uživatelé mají přiřazenou vyšší třídy zdroje nebo SQL Data Warehouse nakonfigurovaný s nízkou DWU. Některé dotazy, jako je DMV dotazů vždy lze spustit.|
| [TempDB][]          | Maximální velikost Tempdb                                | 399 GB na DW100. Proto na DWU1000 Tempdb je bude mít rozměr 3,99 TB |


## <a name="database-objects"></a>Databázových objektů

| Kategorie          | Popis                                  | Maximální            |
| :---------------- | :------------------------------------------- | :----------------- |
| Databáze          | Maximální velikost                                     | 240 TB komprimovány na disku<br/><br/>Zde je nezávislý na tempdb nebo protokolu místa, a proto se snaží zde trvalé tabulek.  Skupinový columnstore komprese odhadovaných na 5 X.  Tato komprese umožňuje databáze, kterou chcete zvětšit přibližně 1 PB při všechny tabulky jsou skupinový columnstore (výchozí typ tabulky).|
| Tabulky             | Maximální velikost                                     | 60 TB komprimovány na disku   |
| Tabulky             | Tabulek na databázi                          | 2 miliard          |
| Tabulky             | Sloupců v tabulce                            | 1 024 sloupců       |
| Tabulky             | Bajtů jeden sloupec                             | Závisí na [typu dat][]sloupce.  Limit je 8000 znaků datových typů 4000 nvarchar nebo 2 GB pro datové typy MAX.|
| Tabulky             | Bajty na řádku, definované velikosti                  | 8 060 bajtů<br/><br/>Počet bajtů na řádek se vypočítá stejným způsobem jako pro systém SQL Server pomocí komprese stránky. SQL Server SQL datový sklad podporuje řádku přetečení úložiště, což umožňuje **proměnné délky sloupců** tak, aby se posune vypnout řádku. Pokud proměnné délky řádky odesílají vypnout řádku, pouze kořenový 24 8bajtový typ je uložená ve hlavního záznamu. Další informace naleznete v článku MSDN [přetečení řádek dat vyšší než 8 KB][] .|
| Tabulky             | Oddíly v tabulce                    | 15 000<br/><br/>Vysoký výkon, doporučujeme minimalizace počet oddílů potřebovat při pořád podpůrné vašim požadavkům pro firmy. Narůstající velikostí počet oddílů, režijních pro jazyk DDL (Data Definition) a Data pro manipulaci s jazyk (DML) operace roste a způsobí, že nižší výkon.|
| Tabulky             | Znaků na hodnotu hranice oddílu.| 4000 |
| Index             | Skupinový nejsou indexy každou tabulku.        | 999<br/><br/>Platí jenom rowstore tabulkami.|
| Index             | Skupinový indexy každou tabulku.            | 1<br><br/>Platí pro rowstore a columnstore tabulkami.|
| Index             | Index velikost klíče.                          | 900 bajtů.<br/><br/>Se týká jenom rowstore indexy.<br/><br/>Indexy datová sloupce s maximální velikosti doručovaných víc než 900 bajtů lze vytvořit, pokud stávající data ve sloupcích není větší než 900 bajtů při vytvoření indexu. Však později vložit nebo aktualizace akce na stránce sloupce, které způsobují celková velikost překročení 900 bajtů se nezdaří.|
| Index             | Klíčové sloupců na index.                   | 16<br/><br/>Se týká jenom rowstore indexy. Skupinový columnstore indexy zahrnout všechny sloupce.|
| Statistiky        | Velikost sloučený sloupec hodnot.      | 900 bajtů.         |
| Statistiky        | Sloupce na Statistika objektu.           | 32                 |
| Statistiky        | Statistiky vytvořil sloupců v tabulce. | 30 000            |
| Uložené procedury | Úrovní vnoření.               | 8                 |
| Zobrazení              | Sloupců v zobrazení                         | použít až 1 024             |


## <a name="loads"></a>Načtení

| Kategorie          | Popis                                  | Maximální            |
| :---------------- | :------------------------------------------- | :----------------- |
| Polybase zatížení    | Bajty na řádku                                | 32 768<br/><br/>Načtení Polybase jsou omezené na menší než 32K načtení řádků obou a nelze načíst VARCHR(MAX), NVARCHAR(MAX) nebo VARBINARY(MAX).  Toto omezení existuje dnes, budou odebrány velmi brzy.<br/><br/>


## <a name="queries"></a>Dotazy

| Kategorie          | Popis                                  | Maximální            |
| :---------------- | :------------------------------------------- | :----------------- |
| Dotaz             | Ve frontě dotazy na uživatelské tabulky.               | 1 000               |
| Dotaz             | Souběžné dotazy v zobrazeních systému.          | 100                |
| Dotaz             | Ve frontě dotazy na systém zobrazení               | 1 000               |
| Dotaz             | Maximální parametry                           | 2098               |
| Dávkové             | Maximální velikost                                 | 65, 536 * 4096        |
| Vyberte výsledků    | Sloupců v jednotlivých řádcích                              | 4096<br/><br/>Můžete nesmějí mít nikdy víc než 4096 sloupců v jednotlivých řádcích ve výsledku vyberte. Je možné, že budete mít vždy 4096. Pokud plán dotazů vyžaduje dočasné tabulky, mohou být platná 1024 sloupců v tabulce maximální.|
| VYBERTE            | Vnořené poddotazy                            | 32<br/><br/>Můžete nesmějí mít nikdy víc než 32 vnořené poddotazy v příkazu SELECT. Je možné, že budete mít vždy 32. Například spojení můžete do plánu dotazu dejte poddotazu. Počet poddotazy lze také omezený dostupnou pamětí.|
| VYBERTE            | Sloupců na spojení                             | 1 024 sloupců<br/><br/>Můžete nesmějí mít nikdy víc než 1 024 sloupce ve spojení. Není nijak zaručené, můžete si vždy 1024. Pokud dočasné tabulku s více sloupců než výsledek spojení, které vyžaduje plán spojení, 1 024 omezení se vztahuje k tabulce dočasné. |
| VYBERTE            | Počet bajtů za Seskupit podle sloupce.                  | 8060<br/><br/>Sloupce v klauzuli GROUP BY může obsahovat maximálně 8 060 bajtů.|
| VYBERTE            | Bajty řadit podle sloupců                   | 8 060 bajtů.<br/><br/>Sloupce v klauzule ORDER BY může obsahovat maximálně 8 060 bajtů.|
| Identifikátory a konstant za údajů | Číslo odkazovaného identifikátorů a konstanty. | 65 535<br/><br/>SQL datový sklad omezuje počet identifikátorů a konstant, které mohou být obsaženy v jediném výrazu dotazu. Toto omezení je 65 535. Vyšší než toto číslo výsledkem chyba serveru SQL 8632. Další informace najdete v tématu [Vnitřní chyba: překročení omezení služeb výraz][].|


## <a name="metadata"></a>Metadata

| Systémové zobrazení                        | Maximální počet řádků |
| :--------------------------------- | :------------|
| Sys.dm_pdw_component_health_alerts | 10 000       |
| Sys.dm_pdw_dms_cores               | 100          |
| Sys.dm_pdw_dms_workers             | Celkový počet zaměstnanců systému správy dokumentů pro poslední 1000 SQL žádostí o. |
| Sys.dm_pdw_errors                  | 10 000       |
| Sys.dm_pdw_exec_requests           | 10 000       |
| Sys.dm_pdw_exec_sessions           | 10 000       |
| Sys.dm_pdw_request_steps           | Celkový počet kroků pro posledních požadavků SQL 1000, které jsou uložené v sys.dm_pdw_exec_requests. |
| Sys.dm_pdw_os_event_logs           | 10 000       |
| Sys.dm_pdw_sql_requests            | Poslední 1000 SQL požadavky, které jsou uložené v sys.dm_pdw_exec_requests. |


## <a name="next-steps"></a>Další kroky
Další informace najdete v článku [Přehled SQL datový sklad][].

<!--Image references-->

<!--Article references-->
[Data skladové jednotky (DWU)]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[Přehled SQL datový sklad]: ./sql-data-warehouse-overview-reference.md
[Správa zátěží na projektu]: ./sql-data-warehouse-develop-concurrency.md
[TempDB]: ./sql-data-warehouse-tables-temporary.md
[datový typ]: ./sql-data-warehouse-tables-data-types.md
[vytváření požadavek podpory můžete]: /sql-data-warehouse-get-started-create-support-ticket.md

<!--MSDN references-->
[Přetečení řádek dat než 8 KB]: https://msdn.microsoft.com/library/ms186981.aspx
[Vnitřní chyba: překročení omezení služeb výraz]: https://support.microsoft.com/kb/913050
