<properties
   pageTitle="Použití Power BI s SQL datový sklad | Microsoft Azure"
   description="Tipy pro používání Power BI s Azure SQL datový sklad k vývoji řešení."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/31/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="use-power-bi-with-sql-data-warehouse"></a>Použití Power BI s SQL datový sklad
Jako s databáze SQL Azure SQL datový sklad přímé připojení umožňuje uživateli využívat výkonných logické přenos směrem dolů vedle analytických funkcí aplikace Power BI.  S přímé připojení dotazy jsou odesílány sklad Azure SQL Data v reálném čase jako zkoumat data.  Tento parametr v kombinaci s škále SQL datový sklad umožňuje uživatelům vytvářet dynamické zprávy v minutách proti TB dat.  Úvod tlačítka Otevřít v Power BI navíc umožňuje uživatelům přímé připojení Power BI k jejich SQL datový sklad bez shromažďování informací z jiných částí Azure.

Při použití přímé připojení prosím poznámky:

+ Zadejte název serveru plně kvalifikovaný při připojování (viz níže další podrobnosti)
+ Zajistěte, aby pravidla brány firewall pro databázi není nakonfigurován "Povolit přístup ke službám Azure".
+ Každá akce například vybrat sloupce nebo přidání filtru bude přímo dotazu datový sklad
+ Dlaždice jsou aktualizovány zhruba každých 15 minut (aktualizace nemusí být naplánované)
+ Služba Q & A není k dispozici pro přímé připojení datové sady
+ Změny schématu nejsou vybraných kontakty automaticky
+ Všechny dotazy přímé připojení vyprší po 2 minuty

Tato omezení a poznámky mohou měnit zdokonalování zkušenosti. Podrobný postup pro připojení je uveden níže.  

## <a name="using-the-open-in-power-bi-button"></a>Pomocí tlačítka "Otevřít v Power BI.
Nejjednodušší způsob, jak přechod mezi SQL datový sklad a Power BI je tlačítka Otevřít v Power BI. Kliknutím na toto tlačítko umožňuje bezproblémové začít vytvářet nový řídicí panely Power BI.  

1.  Začněte tím přejděte na instanci aplikace datový sklad SQL Azure klasické portálu.
2.  Kliknutím na tlačítko "Otevřený v Power BI".
3.  Pokud nám nemůžou přímé přihlášení, nebo pokud nemáte účet Power BI, budete muset přihlásit.  
4.  Budete přesměrováni na stránku připojení SQL datový sklad s informacemi z vaší SQL datový sklad předem vyplněné.
5.  Po zadání přihlašovacích údajů budete plně připojeni k datový sklad SQL.

## <a name="connecting-through-the-power-bi-portal"></a>Připojení prostřednictvím portálu Power BI
Kromě použití tlačítka Otevřít v Power BI, uživatelé mohou také připojit jejich datový sklad SQL pomocí portálu Power BI.

1.  Kliknutím na "Načíst Data" v dolní části navigačního podokna.
2.  Vyberte "Databází".
3.  Jednou na stránce databáze vyberte sklad Azure SQL dat a klikněte na "Připojit".
4.  Zadejte informace potřebné připojení.  Název serveru a název databáze najdete v portálu Azure.
5.  Přesměrovaný zpět na hlavní stránce Power BI a po připojení k je nastavená jako novou položku v části "Datové sady" zobrazí s názvem instanci aplikace.  
6.   Klikněte na tlačítko nové sady dat, které můžete prozkoumat všech tabulek a zobrazení v databázi. Výběr sloupce odešle dotazu zpátky ke zdroji dynamicky vytvářet vizuální. Tyto vizuální prvky můžete uložit do nové sestavy a připnuté zpátky do řídicího panelu.

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop/
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integration/

<!--MSDN references-->

<!--Other Web references-->
