<properties
   pageTitle="Nepřerušený provoz v cloudu – obnovit odstraněnou databázi - databáze SQL | Microsoft Azure"
   description="Informace o obnovení v okamžiku, který umožňuje vrátit databáze SQL Azure k předchozí bodu v čase (až 35 dní)."
   services="sql-database"
   documentationCenter=""
   authors="stevestein"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/01/2016"
   ms.author="sstein"/>

# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Obnovit databázi Azure SQL pomocí zálohy Automatická databáze

Databáze SQL nabízí tři možnosti obnovení databáze pomocí [že databáze SQL automatické zálohy](sql-database-automated-backups.md). Obnovit databázi ze zálohy spuštěná služba jejich [doba uchovávání informací](sql-database-service-tiers.md) do:

- Nové databáze na stejném logické serveru obnovit až určitém bodě v čase v rámci doba uchovávání informací. 
- Databáze na stejném logické serveru obnovit až odstranění dobu odstraněné databáze.
- Nové databáze na libovolném logické serveru v jakékoli oblasti obnovit až posledních denního zálohování v úložišti objektů blob replikovat geo (Vzdálená pomoc GRS).

Můžete také [databáze SQL automatické zálohy](sql-database-automated-backups.md) k vytvoření [databáze zkopírovat](sql-database-copy.md) na libovolném logické serveru v oblasti, která odpovídá využití transakce aktuální databáze SQL. Archivace využití transakce konzistentní kopii databáze služby dlouhodobě úložiště za období uchování nebo přejít na místní kopii databáze můžete použít kopie databáze a [Exportovat BACPAC](sql-database-export.md) nebo Azure OM instance serveru SQL Server.

## <a name="recovery-time"></a>Obnovení času

Obnovení čas obnovení databáze pomocí zálohy Automatická databáze je vliv na několika faktorech: 
 - Velikost databáze
 - Úroveň výkonu databáze
 - Počet transakce protokolů souvisejících
 - Částka aktivity, musí být přehrány obnovit bodu obnovení
 - Šířka pásma při obnovení do jiné oblasti 
 - Počet souběžné obnovení požadavků zpracovávání v cílové oblasti. 
 
 Obnovení databáze velmi velké a/nebo aktivní může trvat několik hodin. Pokud tam je delší výpadku v oblasti, je možné, po které bude velkého množství Geo obnovení požadavků zpracovávaných jiných oblastí. Pokud jsou tu uvedené velký počet požadavků zvýší se sice obnovení dobu databází v dané oblasti. Většinou databáze obnoví dokončeno do 12 hodin.

 Není žádná předdefinované funkce hromadně obnovit. [Databáze SQL Azure: obnovení celého serveru](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) je příklad jedním ze způsobů provedení tohoto úkolu.

> [AZURE.IMPORTANT] Když Pokud chcete obnovit, pomocí automatické zálohy, můžete musíte být členem role přispěvatele SQL serveru v předplatného nebo vlastník předplatného. Na portálu Azure, prostředí PowerShell nebo rozhraní REST API, můžete obnovit. Nelze použít jazyce Transact-SQL. 

## <a name="point-in-time-restore"></a>Obnovení v okamžiku

Obnovení v okamžiku umožňuje obnovení existující databáze jako novou databázi předchozí na stejné logické server [že SQL databáze automatické zálohy](sql-database-automated-backups.md). Nelze přepsat existující databázi. Můžete obnovit předchozí v čase použití [Azure portál](sql-database-point-in-time-restore-portal.md), [prostředí PowerShell](sql-database-point-in-time-restore-powershell.md) nebo [Rozhraní REST API](https://msdn.microsoft.com/library/azure/mt163685.aspx).

> [AZURE.SELECTOR]
- [V okamžiku obnovení: Portál Azure](sql-database-point-in-time-restore-portal.md)
- [Obnovení v okamžiku: prostředí PowerShell](sql-database-point-in-time-restore-powershell.md)

Databáze jde obnovit na úroveň výkonu nebo ohebné fondu. Je nutné ověřit, jestli že máte dostatečnou kvóty DTU na logické serveru nebo pružná fondu. Mějte na paměti, že obnovení vytvoří novou databázi a služby osy a výkonu úroveň obnovené databáze se liší od aktuálního stavu živé databáze. Po dokončení obnovení databáze je normální plně přístupné online databáze Účtovaná sazbou normální založené na úroveň osy a výkonu služby. Náklady vzniknou není až do dokončení obnovení databáze.

Obecně earler bodů pro účely obnovení obnovit databázi. Přitom, můžete považovat obnovené databáze jako náhrada pro databázi nebo použít k načtení dat z a pak aktualizujte původní databáze. 

- ***Databáze náhradní:*** Pokud obnovené databáze slouží k nahrazení původní databáze, měli byste ověřit úroveň výkonu a/nebo vrstvy služeb jsou optimální a změnit velikost databáze v případě potřeby. Můžete přejmenovat původní databáze a pojmenujte obnovené databáze původní pomocí příkazu Vlastnosti databáze v T-SQL. 
- ***Obnovení dat:*** Pokud nebudete chtít k načtení dat z obnovená databáze, kterou chcete obnovit z chybu uživatele nebo aplikace, musíte se samostatně k psaní a spustit skripty obnovení jakéhokoliv dat je potřeba extrahování dat z obnovenou databázi k původní databázi. Ačkoli obnovení může trvat delší dobu, budou zobrazeny v seznamu databází v celém obnovení databáze. Pokud byste odstranili databázi během obnovení, zruší operaci a se nezapočítávají pro databázi, která neproběhla na obnovit. 

Podrobné informace o použití v okamžiku obnovení obnovit uživatele a aplikace chyb najdete v článku obnovení [v daném okamžiku](sql-database-recovery-using-backups.md#point-in-time-restore)

## <a name="deleted-database-restore"></a>Obnovení odstraněné databáze

Obnovení odstraněné databáze můžete obnovit odstraněnou databázi k odstranění dobu odstraněné databáze na stejném logické serveru pomocí [že databáze SQL automatické zálohy](sql-database-automated-backups.md). 

> [AZURE.IMPORTANT] Pokud byste odstranili instance serveru databáze SQL Azure, všechny své databáze budou odstraněny také a není možné obnovit. Nejsou podporovány v současné době obnovení odstraněné serveru.

Můžete použít stejné nebo nový název databáze obnovenou databázi. Můžete použít na [Azure portál](sql-database-restore-deleted-database-portal.md) [prostředí PowerShell](sql-database-restore-deleted-database-powershell.md) nebo [ZBÝVAJÍCÍ (createMode = obnovení)](https://msdn.microsoft.com/library/azure/mt163685.aspx). 

> [AZURE.SELECTOR]
- [Odstranění obnovení databáze: Azure portálu](sql-database-restore-deleted-database-portal.md)
- [Odstranění obnovení databáze: prostředí PowerShell](sql-database-restore-deleted-database-powershell.md)

## <a name="geo-restore"></a>Obnovení GEO

Obnovení GEO umožňuje obnovit databázi SQL serveru v Azure oblasti ze posledních replikovat geo [Automatické denní zálohování](sql-database-automated-backups.md). Obnovení GEO používá geo nadbytečné zálohy jako zdroj a mohou sloužit k obnovení databáze, i když je přístupný z důvodu výpadku databáze nebo datacentra. Můžete použít na [Azure portál](sql-database-geo-restore-portal.md) [prostředí PowerShell](sql-database-geo-restore-powershell.md)nebo [ZBÝVAJÍCÍ (createMode = obnovení)](https://msdn.microsoft.com/library/azure/mt163685.aspx) 

> [AZURE.SELECTOR]
- [GEO-obnovení: Portál Azure](sql-database-geo-restore-portal.md)
- [Obnovení GEO: prostředí PowerShell](sql-database-geo-restore-powershell.md)

Obnovení GEO je výchozí možnost, když databázi není dostupný kvůli incident v oblasti, kde je hostovaný databáze. Pokud incidentu velkém měřítku v oblasti výsledky v nedostupnost databázové aplikace, můžete obnovit Geo obnovení databáze ze zálohy posledních k serveru v jakékoli jiné oblasti. Všechny zálohy jsou replikovat geo a může mít zpoždění mezi při zálohování přijato a geo replikovat na Azure objektů blob v jiné oblasti. Toto zpoždění může být až jednu hodinu v případě selhání může existovat nahoru na 1 hodinu ztrátou dat, například operace RPO až 1 hodinu. Na následujícím obrázku je obnovení databáze z posledního denní zálohování.

![obnovení GEO](./media/sql-database-geo-restore/geo-restore-2.png)

Podrobné informace o použití funkce Geo obnovit z výpadku najdete v článku [obnovení z výpadek](sql-database-disaster-recovery.md)

> [AZURE.IMPORTANT] Obnovení Geo je dostupný u všech úrovní služby, je ale většina základních řešení obnovení havárie k dispozici v databázi SQL s nejdelší operace RPO a odhad využití čas (Vložit). Pro základní databáze maximální velikosti 2 GB Geo-obnovení řešením je vhodnější DR s vložit 12 hodin. Pro větší standardní nebo Premium databáze, pokud výrazně žádoucí kratší dobu obnovení nebo zmenšit pravděpodobnost ztrátou dat zvažte použití aktivní Geo replikace. Aktivní Geo replikace nabízí mnohem nižší operace RPO a vložit pouze vyžaduje zahájit přepojení na nepřetržitě replikovanou sekundární. Další informace najdete v tématu [Aktivní Geo replikace](sql-database-geo-replication-overview.md).

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Programově obnovování pomocí automatického zálohování

Jak je popsáno výše v addiition k portálu Azure obnovení databáze můžete provést zprávu programově pomocí prostředí PowerShell Azure a rozhraní REST API. Následující tabulky popisují sady příkazů k dispozici.

### <a name="powershell"></a>Prostředí PowerShell

|Rutina|Popis|
|------|-----------|
|[Get-AzureRmSqlDatabase](https://msdn.microsoft.com/en-us/library/azure/mt603648.aspx)|Získá jednu nebo více databází.|
|[Get-AzureRMSqlDeletedDatabaseBackup](https://msdn.microsoft.com/en-us/library/azure/mt693387.aspx)|Získá odstraněné databázi, která lze obnovit.|
|[Get-AzureRmSqlDatabaseGeoBackup](https://msdn.microsoft.com/library/azure/mt693388.aspx)|Získá geo záložní kopii databáze.|
|[Obnovení AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390.aspx)|Obnovení databáze SQL.|
||||

### <a name="rest-api"></a>ROZHRANÍ REST API

|ROZHRANÍ API|Popis|
|---|-----------|
|[ZBÝVAJÍCÍ (createMode = obnovení)](https://msdn.microsoft.com/library/azure/mt163685.aspx)|Obnovení databáze|
|[Získání vytvořit nebo aktualizovat stav databáze](https://msdn.microsoft.com/library/azure/mt643934.aspx)|Vrátí stav během obnovení|
||||



## <a name="summary"></a>Souhrn

Automatické zálohování databáze Chraňte uživatele a chyby aplikací, odstranění náhodné databáze a dlouhodobé výpadků. Toto řešení nula náklady nula – správce je dostupný u všech SQL databáze. 

## <a name="next-steps"></a>Další kroky

- Přehled kontinuitu business a scénáře najdete v tématu [Přehled kontinuitu Business](sql-database-business-continuity.md)
- Další informace o databáze SQL Azure automatické zálohy, najdete v článku [automatické zálohy databáze SQL](sql-database-automated-backups.md)
- Další informace o rychlejší možnosti obnovení najdete v tématu [Aktivní Geo replikace](sql-database-geo-replication-overview.md)  
- Další informace o použití zálohy pro automatické archivace najdete v tématu [kopie databáze](sql-database-copy.md)
