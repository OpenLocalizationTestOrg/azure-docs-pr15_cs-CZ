   <properties
   pageTitle="Načtení dat do Azure SQL datový sklad | Microsoft Azure"
   description="Další obvyklé scénáře pro data do SQL datový sklad načítání. Jedná se o pomocí PolyBase, úložišti objektů blob Azure, textových souborů a dodávky disku. Můžete taky pomocí nástrojů jiných výrobců."
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
   ms.date="07/12/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="load-data-into-azure-sql-data-warehouse"></a>Načtení dat do datový sklad SQL Azure

Přehled scénářů možnosti a doporučení pro načítání dat do SQL datový sklad.

Nejtěžší část načítání dat je obvykle Příprava dat pro načíst. Azure zjednodušuje načítání používají úložišti objektů blob Azure běžné úložiště dat pro mnoho služeb a organizovat komunikaci a data pohyb mezi služby Azure pomocí Azure Data Factory. Tyto procesy jsou integrována s technologií PolyBase, který používá datových souběžné zpracování (MPP) k načtení dat paralelně z úložiště objektů blob Azure do SQL datový sklad. 

Podrobné pokyny, které načtení ukázkových databází najdete v článku [načtení ukázkových databází][].

## <a name="load-from-azure-blob-storage"></a>Načtení z úložiště objektů blob Azure
Nejrychlejší způsob, jak importovat data do SQL datový sklad je používat PolyBase k načtení dat z úložiště objektů blob Azure. PolyBase používá k načítání dat paralelně z úložiště objektů blob Azure SQL datový sklad datových souběžné zpracování (MPP) návrh. Použití PolyBase, můžete příkazy T SQL nebo Azure Data Factory kanálu.

### <a name="1-use-polybase-and-t-sql"></a>1. na PolyBase a T-SQL

Přehled procesu načítání:

2. Naformátujte data jako UTF-8, protože PolyBase aktuálně nepodporuje UTF-16.
2. Přesunutí dat do úložišti objektů blob Azure a uložte ho do soubory ve formátu RTF.
3. Konfigurace externí objekty v SQL datový sklad definovat umístění a formátu data
4. Spustí příkaz T SQL k načtení dat paralelně do nové tabulky databáze.

<!-- 5. Schedule and run a loading job. --> 

Kurz najdete v článku [načtení dat z úložiště objektů blob Azure SQL datový sklad (PolyBase)][].

### <a name="2-use-azure-data-factory"></a>2. Azure Data Factory použití

Jednodušší způsob, jak používat PolyBase můžete vytvořit Azure Data Factory kanálem k odesílání zpráv, které používá PolyBase k načtení dat z úložiště objektů blob Azure do SQL datový sklad. Toto je rychlé konfigurace od nemusíte definovat objekty T-SQL. Pokud potřebujete k vytvoření dotazu externí data bez importu, použijte T-SQL. 

Přehled procesu načítání:

2. Naformátujte data jako UTF-8, protože PolyBase aktuálně nepodporuje UTF-16.
2. Přesunutí dat do úložišti objektů blob Azure a uložte ho do soubory ve formátu RTF.
3. Vytvoření kanálu Azure Data Factory k jedí data. Pomocí možnosti PolyBase.
4. Plánování a pořádání kanálu.

Kurz najdete v článku [načtení dat z úložiště objektů blob Azure SQL datový sklad (Azure Data Factory)][].


## <a name="load-from-sql-server"></a>Načtení ze serveru SQL Server
Načtení dat ze serveru SQL Server SQL datový sklad můžete Integration Services (SSIS), převodu textových souborů nebo příjemce disků společnosti Microsoft. Čtení k najdete v článku přehled různých načtení procesy a odkazy na výukové programy.

Plánování migrace úplné dat ze serveru SQL Server SQL datový sklad, najdete v článku [Přehled migrace][]. 

### <a name="use-integration-services-ssis"></a>Použití služby integrace služby SSIS)
Pokud už používáte balíčků Integration Services (SSIS) načíst do SQL Server, můžete aktualizovat balíčků používat SQL serveru jako zdroje a SQL datový sklad jako cíle. Toto je rychlý a snadný, a je dobrá volba, pokud nejsou pokoušíte migrace proces načítání už použít data v cloudu. Kompromis je, že bude nižší než použití PolyBase, protože tato SSIS neprovádí načíst paralelně, načíst.

Přehled procesu načítání:

1. Kontrola balíčku Integration Services tak, aby ukazovaly na instanci systému SQL Server pro tento zdroj a databázi SQL datový sklad pro cíle.
2. Migrace schématu do SQL datový sklad, pokud ještě není tam.
3. Změňte mapování ve vašich balíčků použít pouze datové typy podporované SQL datový sklad.
3. Plánování a pořádání balíček.

Kurz najdete v článku [načtení dat z SQL serveru k Azure datový sklad SSIS (SQL)][].

### <a name="use-azcopy-recommended-for--10-tb-data"></a>Použití AZCopy (doporučeno pro < 10 TB dat)
Je-li vaše data velikost < 10 TB, můžete exportovat data z SQL serveru k textových souborů, zkopírujte soubory k úložišti objektů blob Azure a pomocí PolyBase data načítáte do SQL datový sklad

Přehled procesu načítání:

1. Pomocí nástroje příkazového řádku bcp exportovat data ze serveru SQL Server textových souborů.
2. Použijte nástroj příkazového řádku AZCopy ke kopírování dat z textových souborů k úložišti objektů blob Azure.
3. Použití PolyBase načíst do SQL datový sklad.

Kurz najdete v článku [načtení dat z úložiště objektů blob Azure SQL datový sklad (PolyBase)][].

### <a name="use-bcp"></a>Použití bcp
Pokud máte malou část dat slouží k načtení přímo do Azure SQL datový sklad bcp.

Přehled procesu načítání:
1. Pomocí nástroje příkazového řádku bcp exportovat data ze serveru SQL Server textových souborů.
2. Načtení dat z textových souborů přímo do SQL datový sklad pomocí bcp.

Kurz najdete v článku [načtení dat ze serveru SQL Server Azure SQL datový sklad (bcp)][].


### <a name="use-importexport-recommended-for--10-tb-data"></a>Použití Import a Export (doporučeno pro data > 10 TB)
Pokud chcete přesunout na jiné místo na Azure velikost dat je > 10 TB, doporučujeme použít naše disku dodací služby [Import nebo Export][]. 

Přehled procesu načítání
2. Pomocí nástroje příkazového řádku bcp exportovat data ze serveru SQL Server textových souborů na převoditelné discích.
3. Dodat disků společnosti Microsoft.
4. Microsoft načte data do SQL datový sklad

## <a name="load-from-hdinsight"></a>Načtení z Hdinsightu
SQL datový sklad podporuje načtení dat z Hdinsightu pomocí PolyBase. Proces je stejná jako načtení dat z úložiště objektů Blob Azure - využití PolyBase k připojení k Hdinsightu načíst data. 

### <a name="1-use-polybase-and-t-sql"></a>1. na PolyBase a T-SQL

Přehled procesu načítání:

2. Naformátujte data jako UTF-8, protože PolyBase aktuálně nepodporuje UTF-16.
2. Přesunutí dat do HDInsight a uložte ho do soubory ve formátu RTF, ORC nebo Parquet formát.
3. Konfigurace externí objekty v SQL datový sklad k určení umístění a formát data.
4. Spustí příkaz T SQL k načtení dat paralelně do nové tabulky databáze.

Kurz najdete v článku [načtení dat z úložiště objektů blob Azure SQL datový sklad (PolyBase)][].

## <a name="recommendations"></a>Doporučení

V mnoha partneři mít načítání řešení. Další informace najdete v části Seznam naše [řešení partnerů][]. 

Pokud dat pocházejících z-relační zdroje a chcete načíst do SQL datový sklad budete muset převést na řádky a sloupce před jeho zavedením. Transformovaná data nemusí být uložena v databázi, mohou být uloženy v soubory ve formátu RTF.

Vytvoření statistiky na nově načtená data. Azure SQL datový sklad znamená dosud podpory automaticky vytvořit nebo automatické aktualizace statistiky.  Abyste mohli získávat optimálního výkonu z dotazech, je třeba vytvořit statistiky ve všech sloupcích všech tabulek po první načtení nebo nastanou jakékoliv podstatné změny data.  Podrobnosti najdete v tématu [statistiky][].


## <a name="next-steps"></a>Další kroky
Další tipy vývoj najdete v tématu [Přehled vývoje][].

<!--Image references-->

<!--Article references-->
[Načtení dat z úložiště objektů blob Azure SQL datový sklad (PolyBase)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Načtení dat z úložiště objektů blob Azure SQL datový sklad (Azure Data Factory)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md
[Načtení dat z SQL serveru k Azure datový sklad SSIS (SQL)]: ./sql-data-warehouse-load-from-sql-server-with-integration-services.md
[Načtení dat ze serveru SQL Server Azure SQL datový sklad (bcp)]: ./sql-data-warehouse-load-from-sql-server-with-bcp.md
[Load data from SQL Server to Azure SQL Data Warehouse (AZCopy)]: ./sql-data-warehouse-load-from-sql-server-with-azcopy.md

[Načtení ukázkových databází]: ./sql-data-warehouse-load-sample-databases.md
[Přehled migrace]: ./sql-data-warehouse-overview-migrate.md
[řešení partnerů]: ./sql-data-warehouse-partner-business-intelligence.md
[Přehled vývoje]: ./sql-data-warehouse-overview-develop.md
[Statistiky]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->

<!--Other Web references-->
[Import nebo Export]: https://azure.microsoft.com/documentation/articles/storage-import-export-service/
