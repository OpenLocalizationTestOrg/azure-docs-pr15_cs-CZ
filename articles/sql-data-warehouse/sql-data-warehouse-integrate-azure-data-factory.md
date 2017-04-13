<properties
   pageTitle="Pomocí datový sklad SQL Azure Data Factory | Microsoft Azure"
   description="Tipy pro použití Azure dat Factory (ADF) s Azure SQL datový sklad k vývoji řešení."
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
   ms.date="08/08/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="use-azure-data-factory-with-sql-data-warehouse"></a>Pomocí datový sklad SQL Azure Data Factory

Azure Data Factory poskytuje plně spravované metodu orchestrating přenos dat a spuštění uložené procedury na SQL datový sklad.  To vám umožní snadněji uspořádání a plánování složité extrahovat transformace a načíst (ETL) postupy s SQL datový sklad. Detailní základní informace o Azure Data Factory najdete v článku [si přečtěte následující dokumentaci Azure Data Factory][].

## <a name="data-movement"></a>Přesun dat

Azure Data Factory umožňuje přesun dat mezi zdrojů na pracovišti a různé služby Azure.  Celková, aktuální integrace se službou Azure Data Factory podporuje přesun dat do a z následujících umístění:

+ Úložiště objektů blob Azure
+ Databáze Azure SQL
+ Místní SQL serveru
+ SQL Server na IaaS

Informace o tom, jak nastavit datového kopírovat aktivity najdete v tématu [kopírování dat s Azure Data Factory][]

## <a name="stored-procedures"></a>Uložené procedury
 Stejným způsobem, které mohou sloužit k naplánování přenos dat Azure Data Factory taky slouží k provádění uložené procedury organizovat.  To umožňuje složitější potrubí vytvořit a slouží k rozšíření Azure Data Factory možnost využít výpočetní power SQL datový sklad.

## <a name="next-steps"></a>Další kroky
Přehled integrace naleznete v tématu [Přehled integrace SQL datový sklad][].
Další tipy vývoj najdete v tématu [Přehled vývoje SQL datový sklad][].

<!--Image references-->

<!--Article references-->

[Kopírování dat s Azure Data Factory]: ../data-factory/data-factory-data-movement-activities.md
[Přehled vývoje SQL datový sklad]: ./sql-data-warehouse-overview-develop.md
[Přehled integrace SQL datový sklad]: ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Následující dokumentaci pro Azure Data Factory]:https://azure.microsoft.com/documentation/services/data-factory/

