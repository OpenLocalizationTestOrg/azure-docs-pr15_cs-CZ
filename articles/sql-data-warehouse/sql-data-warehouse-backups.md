<properties
   pageTitle="Zálohování SQL datový sklad | Microsoft Azure"
   description="Informace o zálohování předdefinované databáze SQL datový sklad, které vám umožní obnovte Azure SQL Data Warehouse bod obnovení nebo jinou zeměpisnou oblast."
   services="sql-data-warehouse"
   documentationCenter=""
   authors="lakshmi1812"
   manager="barbkess"
   editor="monicar"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/06/2016"
   ms.author="lakshmir;barbkess"/>

# <a name="sql-data-warehouse-backups"></a>Zálohování SQL datový sklad

SQL datový sklad nabízí místní a zeměpisné zálohování jako součást jeho datový sklad záložní možnosti. Jedná se o Azure úložiště objektů Blob snímky a geo nadbytečné úložiště. Použijte datový sklad zálohy obnovit datový sklad bod obnovení v oblasti hlavní nebo obnovit pro jinou zeměpisnou oblast. Tento článek vysvětluje zvláštnosti zálohy v SQL datový sklad.

## <a name="what-is-a-data-warehouse-backup"></a>Co je sklad zálohování dat?

Zálohování dat sklad jsou data, která slouží k obnovení datový sklad určitou dobu.  Protože SQL datový sklad distribuovaného systému, zálohování dat sklad sestává z větší množství souborů, které jsou uložené v objektů BLOB Azure. 

Zálohování databáze jsou důležitou součástí všech obchodní strategie pro obnovení kontinuitu a havárie, protože jsou ochrana dat náhodné poškozením nebo odstranění. Další informace najdete v tématu [Přehled kontinuitu Business](../sql-database/sql-database-business-continuity.md).

## <a name="data-redundancy"></a>Redundance dat

SQL datový sklad chrání data tak, že ukládání dat v úložišti Premium Azure místně nadbytečné (LRS). Tato funkce úložišti Azure ukládá více kopií synchronní dat v centru místních dat zajistit ochranu průhledné dat pokud jsou lokalizované k chybám. Redundance dat zajišťuje, že vaše data můžete zachován infrastruktury problémy například selhání disku. Redundance dat zajišťuje nepřerušený s odolnost proti chybám a vysoce dostupné infrastrukturu.

Další informace:

- Azure Premium úložiště, najdete v článku [Úvod k úložišti Premium Azure](../storage/storage-premium-storage.md).
- Místně nadbytečné úložiště, najdete v článku [replikace Azure úložiště](../storage/storage-redundancy.md#locally-redundant-storage).


## <a name="azure-storage-blob-snapshots"></a>Snímky úložiště objektů Blob Azure

Jako výhodou používání úložišti Premium Azure SQL datový sklad používá Azure úložiště objektů Blob snímky zálohování datový sklad místně. Obnovení datový sklad bodu obnovení snímek. Snímky spusťte minimálně každých osm hodin a jsou dostupné pro sedmi dnů.  

Další informace:

- Snímky objektů blob Azure, najdete v článku [Vytvoření snímku objektů blob](../storage/storage-blob-snapshots.md).


## <a name="geo-redundant-backups"></a>GEO nadbytečné zálohování

Každých 24 hodin SQL datový sklad ukládá úplné datový sklad standardní úložiště. Úplné datový sklad se vytvoří podle času poslední snímek. Standardní úložiště patří k účtu geo nadbytečné úložiště pro čtení (Vzdálená pomoc GRS). Funkce Azure úložiště Vzdálená pomoc-GRS zreplikuje záložních souborů [párových datacentra](../best-practices-availability-paired-regions.md). Tato geo replikace zajišťuje, že můžete obnovit datový sklad v případě, že nebudete mít přístup k snímky ve vaší primární oblasti. 

Tato funkce je ve výchozím. Pokud nechcete použít geo nadbytečné zálohy, můžete se rozhodnout. 

>[AZURE.NOTE] V Azure úložiště termínů *replikace* odkazuje na kopírování souborů z jednoho místa do jiného. *Replikace databáze* SQL odkazuje na uchovávání k více databázím sekundární sesynchronizovaná s hlavní databází. 

Další informace:
- GEO nadbytečné úložiště, najdete v článku [replikace Azure úložiště](../storage/storage-redundancy.md).
- Vzdálená pomoc GRS úložiště, najdete v článku [geo nadbytečné úložiště přístup pro čtení](../storage/storage-redundancy.md#read-access-geo-redundant-storage).

## <a name="data-warehouse-backup-schedule-and-retention-period"></a>Datový sklad záložní plán a doba uchovávání informací

SQL datový sklad vytvářet snímky na online data sklady každých čtyři až osm hodin a zachová každý snímek sedmi dnů. Můžete obnovit databázi online jeden z úchytů obnovení během posledních sedmi dnů. 

Pokud chcete zobrazit při spuštění poslední snímek, spusťte tento dotaz na online SQL datový sklad. 

```sql
select top 1 *
from sys.pdw_loader_backup_runs 
order by run_id desc;
```

Pokud potřebujete uchovávat snímek po dobu delší než sedmi dny, můžete obnovit bod obnovení nový datový sklad. Po dokončení obnovení se spustí SQL datový sklad vytvářet snímky na nový datový sklad. Pokud nechcete měnit nový datový sklad, snímky zůstat prázdná a tedy minimální náklady na snímku. Může taky najeďte myší databáze, aby SQL datový sklad vytvářet snímky.


### <a name="what-happens-to-my-backup-retention-while-my-data-warehouse-is-paused"></a>Co se stane Moje záložní uchovávání informací, zatímco se pozastaví datový sklad?

SQL datový sklad nevytvoří snímky a nevyprší snímky během pozastavenou datový sklad. Snímek stáří nezmění během pozastavenou datový sklad. Snímek uchovávání informací je založena na počet dní, po které datový sklad je online, ne dnů kalendáře.

Pokud snímek začíná říjen 1 16: 00 a datový sklad je pozastaveno říjen 3 16: 00, je snímek například dvou dnů. Pokaždé, když datový sklad přejde do režimu online je snímek dvou dnů. Pokud má datový sklad online říjen 5 na 16: 00, snímek není dvou dnů a zůstane po pěti další dnů.

Datový sklad vycházejí návrat do online režimu, SQL datový sklad obnoví nové snímky a vyprší jejich platnost snímky, když budou mít více než sedmi dny data.

### <a name="how-long-is-the-retention-period-for-a-dropped-data-warehouse"></a>Jak dlouho je období uchování umísťované datový sklad?
Při přetažení datový sklad uložené 7 dnů datový sklad a snímky a pak odstranit. Můžete obnovit datový sklad kteréhokoli místa uložené obnovit.

> [AZURE.IMPORTANT] Pokud byste odstranili logické instance serveru SQL, všechny databáze, které patří k instanci budou odstraněny také a nelze ji obnovit. Nebude možné obnovit odstraněné serveru.

## <a name="data-warehouse-backup-costs"></a>Datový sklad záložní nákladů

Celkové náklady primární datový sklad a sedmi dnů objektů Blob Azure snímků zaokrouhleno na nejbližší TB. Například pokud datový sklad 1,5 TB a snímky používá 100 GB, je faktura 2 TB dat v úložišti Premium Azure sazby. 

>[AZURE.NOTE] Každý snímek prázdná původně a zvětšuje ho změnit primární datový sklad. Všechny snímky zvětšit jako datový sklad změny. Proto náklady úložiště k snímky zvětšovat podle rychlosti změny.

Pokud používáte geo nadbytečné úložiště, nebudete dostávat poplatků samostatné úložiště. Geo nadbytečné úložiště je účtováno standardní sazbou přístup pro čtení geograficky nadbytečné úložiště (Vzdálená pomoc GRS).

Další informace o SQL datový sklad ceny najdete v článku [SQL datový sklad ceny](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="using-database-backups"></a>Použití zálohování databáze

Primární použití záloh SQL datový sklad je obnovíte datový sklad jeden z úchytů obnovení v období uchování.  

- Obnovení datový sklad SQL, najdete v článku [obnovení datový sklad SQL](sql-data-warehouse-restore-database-overview.md).


## <a name="related-topics"></a>Příbuzná témata

### <a name="scenarios"></a>Scénáře

- Přehled kontinuitu firmy najdete v článku [Přehled kontinuitu Business](../sql-database/sql-database-business-continuity.md)


<!-- ### Tasks -->

- Pokud chcete obnovit datový sklad, najdete v článku [obnovení datový sklad SQL](sql-data-warehouse-restore-database-overview.md)

<!-- ### Tutorials -->

