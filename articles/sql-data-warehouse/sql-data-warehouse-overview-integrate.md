<properties
   pageTitle="Vytvoření integrovaných řešení s SQL datový sklad | Microsoft Azure"
   description="Nástroje a partnery řešení, která integrace s SQL datový sklad. "
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

#<a name="leverage-other-services-with-sql-data-warehouse"></a>Využít jinými službami s SQL datový sklad
Kromě jeho základní funkce SQL datový sklad uživatelům můžete využít řadu další služby v Azure jeho pravém okraji.  Konkrétně jsme vzali aktuálně kroky velký integrovat do těchto věcí:

+ Power BI
+ Azure Data Factory
+ Výukové Azure počítače
+ Azure toku analýzy

Pracujeme s další služby připojení přes Azure ekosystému.

##<a name="power-bi"></a>Power BI
Integrace Power BI umožňuje využívat výpočetního výkonu SQL datový sklad používat dynamické sestavy a vizualizace Power BI. Integrace Power BI momentálně obsahuje:

+ **Přímé připojení**: Vytvoření složitějšího připojení s logické přenos směrem dolů proti SQL datový sklad.  Ve větším měřítku zajistíte rychlejší analýzy.
+ **Otevřít v Power BI**: tlačítka "Otevřít v Power BI" předá instance informace o Power BI umožňuje bezproblémové připojení.

[Integrace s Power BI](./sql-data-warehouse-integrate-power-bi.md) nebo [si přečtěte následující dokumentaci Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) pro další informace v článku.

##<a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory umožňuje uživatelům spravovaných platformu pro vytvářet složité potrubí extrahovat načíst.  SQL datový sklad integrace se službou Azure Data Factory patří:

+ **Uložené procedury**: organizovat spuštění uložené procedury na SQL datový sklad.
+ **Kopie**: použití ADF chcete přesunout data do SQL datový sklad.  Tuto operaci ADF na standardní datové pohyb mechanismus nebo slouží PolyBase na pozadí. 

V tématu [Integrace s Azure Data Factory](./sql-data-warehouse-integrate-azure-data-factory.md) nebo [si přečtěte následující dokumentaci Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) Další informace.

##<a name="azure-machine-learning"></a>Výukové Azure počítače
Azure učení počítače je plně spravovaných analýzy služby, která umožňuje uživatelům vytvářet složité modely využívání velké sadu prediktivní nástrojů.  SQL datový sklad je podporované jako zdrojového a cílového pro tyto modely pomocí následujících funkcí:

+ **Číst Data:** Jednotka modely ve velkém měřítku proti SQL datový sklad pomocí T-SQL.
+ **Zápis dat:** Potvrďte změny z jakéhokoli modelu zpátky na SQL datový sklad.

V tématu [Integrace s Azure počítače výukové](./sql-data-warehouse-integrate-azure-machine-learning.md) nebo [si přečtěte následující dokumentaci Azure počítače výukové](https://azure.microsoft.com/services/machine-learning/) Další informace.

##<a name="azure-stream-analytics"></a>Azure toku analýzy
Azure analýzy toku je složité, plně spravovaných infrastrukturu pro zpracování a jinými data události generované rozbočovači události Azure.  Integrace se službou SQL datový sklad umožňuje pro přenos dat efektivně zpracovat a uložené spolu s relačních dat povolení hlubší a pokročilejší analýzy.  

+ **Úlohy výstup:** Odešlete výstup z toku analýzy úlohy přímo SQL datový sklad.

V tématu [Integrace s Azure toku analýzy](./sql-data-warehouse-integrate-azure-stream-analytics.md) nebo [si přečtěte následující dokumentaci Azure toku analýzy](https://azure.microsoft.com/documentation/services/stream-analytics/) Další informace.

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop/

[Azure Data Factory]: sql-data-warehouse-integrate-azure-data-factory.md
[Azure Machine Learning]: sql-data-warehouse-integrate-azure-machine-learning.md
[Azure Stream Analytics]: sql-data-warehouse-integrate-azure-stream-analytics.md
[Power BI]: sql-data-warehouse-integrate-power-bi.md
[Partners]: sql-data-warehouse-partner-business-intelligence.md

<!--MSDN references-->

<!--Other Web references-->
