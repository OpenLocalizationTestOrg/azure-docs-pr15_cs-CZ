Databázové transakce jednotky (DTU) je měrnou jednotku v SQL databázi, která představuje relativní power databází na základě rozměru reálný: databázové transakce. Jsme trvaly sadu operacích, které jsou typické pro online transakce zpracování požadavku (OLTP) a potom měří kolik transakcí navázáním sekundu v části plně načíst podmínky (to je krátká verze, si můžete přečíst kategorie Podrobnosti v [srovnávací přehled](../articles/sql-database/sql-database-benchmark-overview.md)). 

Například Premium P11 databáze pomocí 1750 DTUs poskytuje 350 x další DTU výpočet power než základní databáze pomocí 5 DTUs. 

![Úvod k databázi SQL: jedna databáze DTUs osy a úroveň.](./media/sql-database-understanding-dtus/single_db_dtus.png)

>[AZURE.NOTE] Při migraci existující databáze SQL serveru, můžete získat odhad úroveň výkonu a služby vrstvy, kterou databáze může vyžadovat v databázi SQL Azure nástrojů jiných výrobců, [Kalkulačky DTU databáze SQL Azure](http://dtucalculator.azurewebsites.net/).

### <a name="dtu-vs-edtu"></a>DTU porovnání eDTU

DTU pro jeden databáze se převádí přímo na eDTU pro pružná databáze. Například databáze do fondu základní pružná databáze nabízí až na 5 eDTUs. To je stejný výkon jako jednu základní databázi. Rozdíl je, že nebude pružná databáze, dokud je používat všechny eDTUs z fondu. 

![Úvod k databázi SQL: pružná fondů ve vrstvě.](./media/sql-database-understanding-dtus/sqldb_elastic_pools.png)

Jednoduchý příklad pomůže. Prohlédněte fond základní pružná databáze s 1000 DTUs a přetáhnout 800 databází v nich. Dokud použily jenom 200 800 databází kdykoli v čase (5 DTU × 200 = 1000), nebude přístupů objemu fondu a nebude snížit výkon databáze. V tomto příkladu je zjednodušené pro projekty. Skutečné matematické je trochu složitější. Na portálu znamená výpočtů pro vás a chce doporučení založené na použití historických databáze. V tématu [aspektech cena a výkonu pro fond pružná databáze](../articles/sql-database/sql-database-elastic-pool-guidance.md) se dozvíte, jak fungují doporučení a výpočty sebe. 
